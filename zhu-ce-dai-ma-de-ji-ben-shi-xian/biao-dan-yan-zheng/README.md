# 表单验证

{% embed url="https://wp0577.gitbook.io/test1/front-end/jqeury/jqueryvalidate-cha-jian-biao-dan-xiao-yan" %}

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

