# 后端配置

通过在baseDao中实现executeUpdate方法可以使用多种用途的update格式。



第一步：在UserAction中创建修改密码方法editPassword

                  /\*\*

                   \* 修改当前用户密码

                   \* **@throws** IOException

                   \*/

                  **public** String editPassword\(\) **throws** IOException{

                                    String f = "1";

                                    //获取当前登录用户

                                    User user = BOSUtils.getLoginUser\(\);

                                    **try**{

                                                      userService.editPassword\(user.getId\(\),model.getPassword\(\)\);

                                    }**catch**\(Exception e\){

                                                      f = "0";

                                                      e.printStackTrace\(\);

                                    }

                                    ServletActionContext.getResponse\(\).setContentType\("text/html;charset=utf-8"\);

                                    ServletActionContext.getResponse\(\).getWriter\(\).print\(f\);

                                    **return** **NONE**;

                  }

第二步：在UserService中提供修改密码方法

                  /\*\*

                   \* 根据用户id修改密码

                   \*/

                  **public** **void** editPassword\(String id, String password\) {

                                    //使用MD5加密密码

                                    password = MD5Utils.md5\(password\);

                                    userDao.executeUpdate\("user.editpassword", password,id\);

                  }

第三步：在BaseDao中提供通用更新方法

                  //执行更新

                  **public** **void** executeUpdate\(String queryName, Object... objects\) {

                                    Session session = **this**.getSessionFactory\(\).getCurrentSession\(\);

                                    Query query = session.getNamedQuery\(queryName\);

                                    **int** i = 0;

                                    **for** \(Object object : objects\) {

                                                      //为HQL语句中的？赋值

                                                      query.setParameter\(i++, object\);

                                    }

                                    //执行更新

                                    query.executeUpdate\(\);

                  }

第四步：在User.hbm.xml中定义更新语句

&lt;query name="user.editpassword"&gt;

                  UPDATE User SET password = ? WHERE id = ?

&lt;/query&gt;

