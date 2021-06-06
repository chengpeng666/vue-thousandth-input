<template>
	<el-input
		type="text"
		v-model="thousandValue"
		:placeholder="placeholder"
		:id="idSet"
		@input="onInputEnter"
		@blur="onBlur"
		@focus="onFocus"
	>
		<template slot="prepend" v-if="showPrepend&&prepend!==''">{{prepend}}</template>
	</el-input>
</template>

<script>
    export default {
        name: 'vue-thousandth-input',
        data() {
            return {
                thousandValue: '',
                returnNum: '',
                trueVal: '',
                prepend: '',
            }
        },
        props: {
            value: {  // 默認表單值，v-model值
                default: ''
            },
            maxAfterPoint: Number,// 保留几位小数点，默认不限制，为0的时候不显示小数点
            maxBeforePoint: Number,//保留小数点前，默认不限制。
            idSet: String, // 给当前输入框添加id
            placeholder: { // 自定义当前输入框的提示語
                type: String,
                default: ''
            },
            showPrepend: {
                type: Boolean,
                default: false
            }
        },

        created() {

            this.value && this.onInputEnter(this.thousandValue = this.comdify(this.value));
            this.thousandValue && this.showPrepend && (this.prepend = this.getPrepend(this.thousandValue));
        },

        methods: {
            /**
             * 输入框的输入事件
             */
            onInputEnter(e) {
                // 如果直接输入的是.
                if (e === '.') {
                    this.thousandValue = '0.'

                    // 如果删除了，只有0了，则清空输入框
                } else if (e === '0') {
                    this.thousandValue = '0'
                }
                // 验证金额输入只能为数字,以及一位小数点
                if (this.maxAfterPoint !== 0) {
                    this.thousandValue = this.thousandValue.replace(/[^\d.]/g, '');
                    // 验证金额输入只能为正数
                } else {
                    this.thousandValue = this.thousandValue.replace(/[^\d]/g, '');
                }

                // 判断输入几位小数
                this.trueVal = this.thousandValue;

                // 如果有小数点位数则返回真实的数据,可以提前判断是否需要保留小数点位数
                this.trueVal = this.returnTrueNum(this.thousandValue);

                // 千分符的转化
                this.thousandValue = this.comdify(this.trueVal);
                // 得到前缀提示
                this.showPrepend && (this.prepend = this.getPrepend(this.thousandValue));

                // 监听返回的数据
                this.$emit('change', {
                    // event: e, // 返回的当前event
                    thousandValue: this.thousandValue, // 返回的当前转换后的value
                    value: this.trueVal // 返回没有转化后的value
                });

                // 绑定v-model
                this.$emit('input', this.trueVal)
            },

            /**
             * 获取焦点
             */
            onFocus(e) {
                this.$emit('focus', {
                    event: e, // 返回的当前event
                    thousandValue: this.thousandValue, // 返回的当前转换后的value
                    value: this.trueVal // 返回没有转化后的value
                });
            },

            /**
             * 失去焦点
             */
            onBlur(e) {
                this.$emit('blur', {
                    event: e, // 返回的当前event
                    thousandValue: this.thousandValue, // 返回的当前转换后的value
                    value: this.trueVal // 返回没有转化后的value
                });
            },

            /**
             * 返回转化后真实的值
             */
            returnTrueNum(n) {
                if (!n) return n;
                let pointStr = n.split('.');
                let num1 = pointStr[0];
                let num2 = pointStr[1];
                let bNum = '';
                let aNum = '';
                if (this.maxBeforePoint && num1 && num1.length) {
                    bNum = `${num1.slice(0, this.maxBeforePoint)}`;
                    // 返回正常的数据
                } else {
                    if(num1===undefined||num1===''){
                        bNum = '0';
										}else {
                        bNum = num1;
                    }
                }
                // 保留几位小数
                if (this.maxAfterPoint && num2 && num2.length > this.maxAfterPoint) {
                    aNum = `${num2.slice(0, this.maxAfterPoint)}`;
                    // 返回正常的数据
                } else {
                    if(num2===undefined){
                        return `${bNum}`;
										}
                    aNum = num2;
                }
								return `${bNum}.${aNum}`;
            },
            getPrepend(n) {
                let count = n.split(',').length - 1;
                let prepends = ['thousand', 'million', 'billion', 'trillion', 'quadrillion'];
                if (count <= 0) {
                    return '';
                } else {
                    return prepends[count - 1];
                }
            },

            /**
             * 千分符的转化
						 * todo 组件初始化时的数据格式转化及四舍五入
             */
            comdify(n) {
                if (!n) return n;
                let str = n.toString().split('.');
                let re = /\d{1,3}(?=(\d{3})+$)/g;
                let n1 = str[0].replace(re, "$&,");
                let n2 = str[1];
                if (n2 && n2.length > 0) {
                    n2 = n2.replace(re, "$&,")
                }
                // 返回值中，注意当只有一个小数点时，且小数点后面没有数字时（也就是n2===''），要显示小数点
                return str.length > 1 && (n2 || n2 === '') ? `${n1}.${n2}` : `${n1}`;
            }

        }

    }
</script>
