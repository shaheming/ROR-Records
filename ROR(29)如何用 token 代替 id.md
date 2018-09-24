# 如何用 token 代替 id

用加密的网址避免别人通过id就可以才想出你产品的流水数量，方法就是增加一个栏位加做token。
我们这里的订单表为orders，每当order生成就是随机生成一个token，这个token是一组乱码。产生乱码的方法

```ruby
before_create :generate_token
  def generate_token
    self.token = SecureRandom.uuid
  end
```

一个串联的系统源头采用token的话，参数要全部用token来传递。

从上一个action跳转到pay含参数(@order.token)`redirect_to pay_order_path(@order.token)`

对接views相应的页面，给pay action传递id为`@resume.order.token`
`<%= link_to pay_order_path(id:@resume.order.token), class: "btn btn-primary pull-center" do %>`

通过上面的步骤让def pay中的order可以被找到
`@order = Order.find_by_token(params[:id])`

pay view有一个表单要填写，同样传递参数`@order.token`让def pay_submit `@order = Order.find_by_token(params[:id])`里面的order可以被找到。
`<%= simple_form_for(@order, url: pay_submit_order_path(@order.token), method: :post) do |f| %>`