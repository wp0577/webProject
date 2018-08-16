# 表单验证

{% embed data="{\"url\":\"https://wp0577.gitbook.io/test1/front-end/jqeury/jqueryvalidate-cha-jian-biao-dan-xiao-yan\",\"type\":\"link\",\"title\":\"JQuery-validate插件（表单校验） - JavaEE\",\"icon\":{\"type\":\"icon\",\"url\":\"https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/spaces%2F-LFcIEEpyyi7v-mjfE-X%2Favatar.png?generation=1529684491520080&alt=media\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://www.gitbook.com/share/space/thumbnail/-LFcIEEpyyi7v-mjfE-X.png\",\"aspectRatio\":0}}" %}

```text
$(function () {
        $("#myForm").validate({
            rules: {
                username: {
                    required: true,
                    minlength: 2
                },
                password: {
                    required: true,
                    rangelength: [6, 12]
                },
                repassword: {
                    required: true,
                    rangelength: [6, 12],
                    equalTo: "[name='password']"
                },
                email: {
                    required: true,
                    email: true
                },
                sex: {
                    required: true
                }
            },
            messages: {
                username: {
                    required: "用户不能为空",
                    minlength: "不得少于2个字符"
                },
                password: {
                    required: "密码不能为空",
                    rangelength: "密码长度6-12位"
                },
                repassword: {
                    required: "密码不能为空",
                    rangelength: "密码长度6-12位",
                    equalTo: "两次密码不一致"
                },
                email: {
                    required: "邮箱不能为空",
                    email: "邮箱格式不正确"
                }
            }
        })
    })

```

