# 服务器规划



Localhost:         

| Manager Service | 8080     | Manager Web | 8081 |
| :--- | :--- | :--- | :--- |
| Content Service | 8083 | Portal Web | 8082 |
| Search Service | 8084 | Search Web | 8085 |
| SSO Service | 8087 | Item Web | 8086 |
| Cart Service | 8090 | SSO Web | 8088 |
| Order Service | 8092 | Cart Web | 8089 |
|  |  | Order Web | 8091 |



项目                                                                 服务器数量                                                                  虚拟机                         ip

----------------------------------------------------------------------------------------------------------------------

Mysql                                                                           2                                                                                              1                                  134

Solr                                                                                          7                                                                                              1                                  154

Redis                                                                            6                                                                                              1                                  153

图片服务器                                                                  2                                                                                              1                                  133

Nginx                                                                           2                                                                                              1                                  141

注册中心                                                                      3                                                                                              1                                  167

Activemq                                                                                  2                                                                                              1                                  168

mall-manager                 8080

mall-content                              8081

mall-search                                8082                                                                                                    

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~            1                                  135

mall-sso                         8080

mall-order                                 8081

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~            1                                  136

mall-manager-web          8080

mall-portal-web             8081

mall-search-web             8082

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~            1                                  137

mall-item-web               8080

mall-sso-web                 8081

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~            1                                  138

mall-cart-web                8080

mall-order-web              8081

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~            1                                  139

