# 商品删改上下架功能

代码略

{% hint style="info" %}
因为tb\_item\_desc表格中，item\_desc字段的type是text，用逆向工程生成的mapper.xml代码如下



```text
<update id="updateByPrimaryKeyWithBLOBs" parameterType="com.wp.pojo.TbItemDesc" >
  update tb_item_desc
  set created = #{created,jdbcType=TIMESTAMP},
    updated = #{updated,jdbcType=TIMESTAMP},
    item_desc = #{itemDesc,jdbcType=LONGVARCHAR}
  where item_id = #{itemId,jdbcType=BIGINT}
</update>
<update id="updateByPrimaryKey" parameterType="com.wp.pojo.TbItemDesc" >
  update tb_item_desc
  set created = #{created,jdbcType=TIMESTAMP},
    updated = #{updated,jdbcType=TIMESTAMP}
  where item_id = #{itemId,jdbcType=BIGINT}
</update>
```

 此处生成的dao文件将会有两个方法：



```text
int updateByPrimaryKeyWithBLOBs(TbItemDesc record);

int updateByPrimaryKey(TbItemDesc record);
```

 根据.xml可知，要更新item\_desc字段，必须执行第一个方法。
{% endhint %}

