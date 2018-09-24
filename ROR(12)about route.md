# ROR(12) |  Paths and URLs from Code

#### Generating Paths and URLs from Code

You can also generate paths and URLs. If the route above is modified to be:

and your application contains this code in the controller:

and this in the corresponding view:

`<%= link_to 'Patient Record', patient_path(@patient) %>`

then the router will generate the path `/patients/17`. This reduces the brittleness of your view and makes your code easier to understand. Note that the id does not need to be specified in the route helper.

photo_path(:id) returns /photos/:id (for instance, photo_path(10) returns /photos/10)

具体的guid 可以参考如下的链接。

http://guides.rubyonrails.org/routing.html
教程里面有一章的写法
https://fullstack.xinshengdaxue.com/posts/226
Step 2: 删除购物车内某一商品

` <%= link_to cart_item_path(cart_item.product_id), method: :delete do %>`
等价于
` <%= link_to cart_item_path(cart_item.product), method: :delete do %>`