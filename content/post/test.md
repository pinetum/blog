+++
categories = ["test"]
date = "2017-04-18T07:23:23+08:00"
description = "just a test."
tags = ["test"]
thumbnail = ""
title = "測試文"
images = [
  "https://source.unsplash.com/-09QE4q0ezw/2000x1322"
]
+++


### CH13 Web Server Development
---
本章學習重點
- 建立可存取的路徑
- 對可存取的路徑做權限管控
- 取得傳入的參數
- 對已存在的handler修改
- 使用RPC API

#### 建立可存取的路徑
例如：`http://127.0.0.1:8069/my_module/books`

1. 於controllers/__init__.py加入`import main`
2. 於__init__.py加入`import controllers`
3. 建立controllers/main.py


```python
from openerp import http
from openerp.http import request
class Main(http.Controller):
    @http.route('/my_module/books', type='http', auth='none')
    def books(self):
        records = request.env['library.book'].sudo().search([])
        result = '<html><body><table><tr><td>'
        result += '</td></tr><tr><td>'.join(
                    records.mapped('name'))
        result += '</td></tr></table></body></html>'
        return result
    @http.route('/my_module/books/json', type='json',
               auth='none')
    def books_json(self):
        records = request.env['library.book']\
                        .sudo().search([])
        return records.read(['name'])
```

- `@http.route`:decorator，用來告知odoo此method是web accessible，且可以將多個網址路徑對應到同一個method中
- `return something`:根據decorator中定義的type需要回傳的物件會有所不同
  - `type='http'`:直接回傳字串，一般來說為HTML，或`request.render('template_name', {datas})`藉由template產生UI

  - `type='json'`:直接回傳物件，decorator會幫忙轉換成json格式
- `request`:為一靜態物件，會包含所有handled request的所有資訊其中`request.env`與model中的`self.env`是相同的
- SESSION:
```python
request.session['hello'] = 'world'
request.session.get('hello')
```


#### 對可存取的路徑做權限管控

修改`@http.route`中的auth參數
- `auth='none'`: 沒有任何驗證機制(user record is always empty)
- `auth='public'`: 假如想讓登入與未登入者都可以使用(user record is set to base.public_user)
- `auth='user'`(default): 只有登入的使用者可以使用


#### 取得傳入的參數
```python
@http.route('/my_module/book_details', type='http',
               auth='none')
def book_details(self, book_id):
    record = request.env['library.book']
                    .sudo().browse(int(book_id))
    return u'<html><body><h1>%s</h1>Authors: %s' % (
        record.name,  u', '.join(
        record.author_ids.mapped('name')) or 'none',
    )
@http.route("/my_module/book_details/<model(\
                       'library.book'):book>",
                       type='http', auth='none')
def book_details_in_path(self, book):
    return self.book_details(book.id)
```
- `/my_module/book_details?book_id=1` for `book_details`
- `/my_module/book_details/1` for `book_details_in_path`

#### 對已存在的handler修改

1. 對Qweb template做replace
2. 對要送給qweb的參數做修改
3. 如果handler有接收參數，可以對參數做前置處理


```xml
 <?xml version="1.0" encoding="UTF-8"?>
       <odoo>
         <template id="show_website_info"
           inherit_id="website.show_website_info">
           <xpath expr="//dl[@t-foreach='apps']" position="replace">
         <table class="table">
           <tr t-foreach="apps" t-as="app">
             <th>
               <a t-att-href="app.website">
               <t t-esc="app.name" /></a>
             </th>
             <td><t t-esc="app.summary" /></td>
           </tr>
         </table>
       </xpath>
     </template>
   </odoo>
```

controllers/main.py:

```python
from openerp import http
from openerp.addons.website.controllers.main import Website


    class Website(Website):
        @http.route()
        def website_info(self):
            result = super(Website, self).website_info()
            result.qcontext['apps'] = \
            result.qcontext['apps'].filtered(
                lambda x: x.name != 'website')
            return result

```

只會看對被過濾的app清單

#### 使用RPC API
1. XMLRPC
  - 每次的呼叫都需要使用者ID與密碼
  - 藉由路徑`/xmlrpc/2/object`使用`execte_kw`呼叫model中的method
2. JSONRPC
  - 僅需要於第一次先呼叫`/web/session/authenticate`往後的呼叫就不需再傳送使用者ID與密碼資訊(驗證資訊紀錄在HTTP Header)
  - odoo UI部分 幾乎都使用此發法 例如 `rpc.call()`, `model.method()` 等


範例:取得所有已安裝的module

使用 XMLRPC
```python
 #!/usr/bin/env python2
import xmlrpclib
db = 'odoo9'
user = 'admin'
password = 'admin'
uid = xmlrpclib.ServerProxy(
    'http://localhost:8069/xmlrpc/2/common')
    .authenticate(db, user, password, {})
odoo = xmlrpclib.ServerProxy(
    'http://localhost:8069/xmlrpc/2/object')
installed_modules = odoo.execute_kw(
    db, uid, password, 'ir.module.module', 'search_read',
    [[('state', '=', 'installed')], ['name']], {'context':
    {'lang': 'fr_FR'}})
for module in installed_modules:
    print module['name']


```
使用 JSONRPC
```python
 #!/usr/bin/env python2
import json
import urllib2
db = 'odoo9'
user = 'admin'
password = 'admin'
request = urllib2.Request(
    'http://localhost:8069/web/session/authenticate',
    json.dumps({
        'jsonrpc': '2.0',
        'params': {
            'db': db,
            'login': user,
            'password': password,
}, }),
    {'Content-type': 'application/json'})
result = urllib2.urlopen(request).read()
result = json.loads(result)
session_id = result['result']['session_id']
request = urllib2.Request(
        'http://localhost:8069/web/dataset/call_kw',
        json.dumps({
        'jsonrpc': '2.0',
        'params': {
            'model': 'ir.module.module',
            'method': 'search_read',
            'args': [
            [('state', '=', 'installed')],
['name'], ],
            'kwargs': {'context': {'lang': 'fr_FR'}},
        },
}), {
        'Content-type': 'application/json',
    })
    result = urllib2.urlopen(request).read()
    result = json.loads(result)
    for module in result['result']:
    print module['name']


```


