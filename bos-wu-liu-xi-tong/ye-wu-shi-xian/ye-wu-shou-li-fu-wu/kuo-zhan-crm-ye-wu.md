# 扩展crm业务

## 1. 通过电话查询客户

```text
 @Override
    public Customer getCustomerByTelephone(String telephone) {
        String sql = "select * from t_customer where telephone = ?";
        List<Customer> list = jdbcTemplate.query(sql, new RowMapper<Customer>(){
            public Customer mapRow(ResultSet rs, int arg1) throws SQLException {
                int id = rs.getInt("id");//根据字段名称从结果集中获取对应的值
                String name = rs.getString("name");
                String station = rs.getString("station");
                String telephone = rs.getString("telephone");
                String address = rs.getString("address");
                String decidedzone_id = rs.getString("decidedzone_id");
                return new Customer(id, name, station, telephone, address, decidedzone_id);
            }
        },telephone);
```

## 2. 通过取件地址查询定区id

不能通过中文关键字进行查询

英文查询中因为会有空格，需要多加一个双引号。

```text
   @Override
    public String getDecidedIdbyAddress(String address) {
        //无法使用中文条件查询
        address = ""+address+"";
        String sql = "select decidedzone_id from t_customer where address = ?";
        List<String> certs = jdbcTemplate.queryForList(sql, String.class, address);
        if(certs.size() == 0) return null;
        return certs.get(0);

    }
```

