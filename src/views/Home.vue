<template>
    <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80">
        <FormItem label="项目名称" prop="name">
            <Input v-model="formValidate.name" placeholder="请输入项目名称"></Input>
        </FormItem>
        <FormItem label="单机数选择" prop="num">
            <Select v-model="formValidate.num" placeholder="请选择单机数" @on-change="getNum">
                <Option value="1">1</Option>
                <Option value="2">2</Option>
                <Option value="3">3</Option>
                <Option value="4">4</Option>
                <Option value="5">5</Option>
            </Select>
        </FormItem>
        <FormItem
                v-for="(item, index) in formValidate.items"
                :key="index"
                :label="'单机数 ' + item.index"
                :prop="'items.' + index + '.value'"
                :rules="{required: true, message: '单机名 ' + item.index +' 不能为空', trigger: 'blur'}">
            <Row>
                <Col span="18">
                    <Input type="text" v-model="item.value" placeholder="请输入单机名"></Input>
                </Col>
            </Row>
        </FormItem>
        <FormItem>
            <Button type="primary" @click="handleSubmit('formValidate')">立即新建</Button>
            <Button @click="handleReset('formValidate')" style="margin-left: 8px">重置</Button>
        </FormItem>
    </Form>
</template>
<script>
    export default {
        name:'home',
        data () {
            return {
                formValidate: {
                    name: '',
                    num:'',
                    items:[]
                },
                ruleValidate: {
                    name: [
                        { required: true, message: '项目名称不能为空', trigger: 'blur' }
                    ],
                    num: [
                        { required: true, message: '请选择单机数', trigger: 'change' }
                    ],
                }
            }
        },
        methods: {
            handleSubmit (name) {
                this.$refs[name].validate((valid) => {
                    if (valid) {
                        this.$Message.success('Success!');
                        var zz=new Object();
                        zz.name=this.formValidate.name;
                        zz.type=this.formValidate.name;
                            zz.text=this.formValidate.name;
                            zz.children=[];
                            this.$store.state.menuItems[0].children.push(zz);
                        for(var i=0;i<this.formValidate.items.length;i++){
                            var key=new Object();
                            key.name='t1';
                            key.type=this.formValidate.items[i].value;
                            key.text=this.formValidate.items[i].value;
                            this.$store.state.menuItems[0].children[0].children.push(key);
                        }
                    } else {
                        this.$Message.error('Fail!');
                    }
                })
            },
            handleReset (name) {
                this.$refs[name].resetFields();
            },
            getNum(e){
                this.formValidate.items=[];
                for(var i=0;i<e;i++){
                    this.formValidate.items.push({
                        value: '',
                        index: i+1,
                    });
                }
            }
        }
    }
    
</script>
