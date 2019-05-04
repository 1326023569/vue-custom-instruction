<template>
   <div>
    <ul>
        <li>
            <input id="name" type="text" v-test.required="formData.name" v-model.lazy="formData.name.value">
        </li>
        <li>
            <input id="password" type="text"  v-test.required="formData.password" v-model.lazy="formData.password.value">
        </li>
        <li>
            <input id="email" type="text"  v-test="formData.email" v-model.lazy="formData.email.value">
        </li>
        <li>
            <button @click="fn">提交</button>
        </li>
    </ul>
   </div>
</template>

<script>
    export default {
        name: 'app',
        data() {
            return {
                formData: {
                    name: {
                        value: '',
                        reg: /^[a-zA-Z]{4,10}$/,
                        msg: '你输入的用户名格式不正确，为英文',
                        focus: false,
                        state: false,
                    },
                    password: {
                        value: '',
                        reg: /^[0-9]{4,6}$/,
                        msg: '你输入的密码格式不正确，为数值',
                        focus: false,
                        state: false,
                    },
                    email: {
                        value: '',
                        reg: /^([0-9]{6,10})@qq\.com$/,
                        msg: '你输入的邮箱格式不正确，为qq邮箱',
                        state: true
                    }
                }
            };
        },
        methods: {
            fn() {
                var state = Object.values(this.formData).every((i) => {
                    return i.state;
                })
                console.log(state)
            }
        },
        directives: {
            test: {
                bind(el, binding) {
                    el.onfocus = function() {
                        binding.value.focus = true;
                    }
                },
                componentUpdated(el, binding) {
                    var config = binding.value;
                    var modifiers = binding.modifiers.required
                    var $parent = el.parentNode;

                    function clear() {
                        var $$p = $parent.getElementsByTagName('p')[0];
                        if ($$p) {
                            $parent.removeChild($$p)
                        }
                    }

                    function testReg() {
                        clear()
                        var state = config.reg.test(config.value);
                        if (!state) {
                            var $p = document.createElement('p');
                            $p.innerHTML = config.msg;
                            $parent.appendChild($p);
                            config.state = false;
                        } else {
                            config.state = true;
                        }
                    }

                    function testRequired() {
                        if (config.focus) {
                            clear()
                            if (modifiers) {
                                var $p = document.createElement('p');
                                $p.innerHTML = '必填项';
                                $parent.appendChild($p);
                                config.state = false;
                            }

                        }
                    }

                    function testNull() {
                        if (config.value) {
                            testReg()
                        } else {
                            testRequired()
                        }
                    }
                    testNull()
                }
            }
        },
    };
</script>
<style>
    div {
        margin: 0 auto;
    }
</style>