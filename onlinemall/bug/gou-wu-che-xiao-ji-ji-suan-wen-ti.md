# 购物车小计计算问题

{% hint style="info" %}
当改变商品数量时，小计和总计的价格会随之改变
{% endhint %}



```javascript
refreshSinglePrice : function(itemId, num){ //重新计算单价
//对所有class为totalprice的元素进行遍历
      $(".totalprice").each(function (i,e) {
       //将元素变为jquery对象
          var _this = $(e);
          //alert(eval(_this.attr("itemId")));
   //取与更改时的商品id相同的对象
          if(eval(_this.attr("itemId") == itemId)) {
           //_this.val((eval(_this.attr("itemPrice"))) / 100 *num);
              var total = eval(_this.attr("itemPrice"));
              //alert(total);
              _this.html(new Number(total/100 * num).toFixed(2)).priceFormat({ //价格格式化插件
                  prefix: '¥',
                  thousandsSeparator: ',',
                  centsLimit: 2
              });
   }
      })
  },
```

 

```text
<div class="pItem pSubtotal">
   <span id="total_price" itemId="${cart.id}" itemPrice="${cart.price}" class="totalprice">¥<fmt:formatNumber groupingUsed="false" value="${cart.price / 100 * cart.num}" maxFractionDigits="2" minFractionDigits="2"/></span>
</div>
```

 

 

