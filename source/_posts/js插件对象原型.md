---
title: js插件对象原型
date: 2016-04-15 15:40:19
tags: js
---

基于requirejs的js控件基类
github：https://github.com/milolu/blog/tree/master/jq-widget

```javascript
/**
 * Created by Silence on 2014/10/27.
 * Describe
 */

define(function(){
    function Widget(){
        this.boundingBox = null;    //属性，最外层容器
    }
    Widget.prototype = {
        //绑定事件
        on : function(type, handler){
            if(typeof this.handlers[type] == "undefined"){
                this.handlers[type] = [];
            }
            this.handlers[type].push(handler);
            return this
        },
        //调用绑定事件
        fire : function(type, data){
            if(this.handlers[type] instanceof Array){
                var handlers = this.handlers[type];
                for(var i = 0; i < handlers.length; i++){
                    handlers[i](data);
                }
            }
        },
        //渲染组件
        render : function(container){
            this.renderUI();
            this.handlers = {};
            this.bindUI();
            this.syncUI();
            //$(container || document.body).append(this.boundingBox);
        },
        //销毁组件
        destroy : function(){
            this.destructor();
            this.boundingBox.off();
            this.boundingBox.remove();
        },
        //获取元素本身
        self : function(){
            return this.boundingBox;
        },
        //获取宽度
        width : function(){
            return this.boundingBox.width();
        },
        //获取高度
        height : function(){
            return this.boundingBox.height();
        },
        //添加dom节点
        renderUI : function(){},
        //监听事件
        bindUI : function(){},
        //初始化组件属性
        syncUI : function(){},
        //销毁前的处理函数
        destructor : function(){}

    };
    return {
        Widget : Widget
    }
});

```
选择控件实现样例
```javascript
/**
 * Created by Silence on 2014/10/30.
 * Describe 选择控件
 * 参数
 *  parent ：父节点（"#parent"）
 *  width：选择框的大小（除标题外的宽度）
 *  css：样式
 *  skinClassName : 皮肤类名
 *  values ：可选择项
 *  onSelect(index, data) ： 选择事件
 *  defaultSel ： 默认选中项
 *  title：选择器的标题
 *
 * API：
 *  setSelectIndex(index, isCallBack):  设置选中项（选中序号，是否进行选中事件，默认不调用）
 *  getSelectValue： 获取当前选中序号及内容
 */

define(["widget"], function(widget){
    function Select(){
        this.cfg = {
            parent : null,
            width : null,
            title : null,
            css : null,
            skinClassName : null,
            values : ["sel1", "sel2", "sel3", "sel4"],
            key : null,
            onSelect : null,
            defaultSel : 0,
            firstCallBack : true,
            enable : true
        };
    }
    Select.prototype = $.extend({}, new widget.Widget(), {
        create : function(cfg){
            $.extend(this.cfg, cfg);
            this._select = this.cfg.defaultSel;
            this.render();
            return this;
        },
        renderUI : function(){
            var htm = "<div class='w-select-box'>{{if title}}<span class='w-select-title'>${title}</span>{{/if}}<div class='w-select ${skinClassName}'><span class='w-select-value'></span>" +
                "<div class='w-select-btn'></div>" +
                "<div class='w-select-values'><ul></ul></div>" +
                "</div></div>";

            this.boundingBox = $($.tmpl(htm, this.cfg));
            var $ul = this.boundingBox.find("ul");
            for(var i=0; i<this.cfg.values.length; i++)
            {
                if(this.cfg.key != null)
                    $ul.append("<li>"+ this.cfg.values[i][this.cfg.key] +"</li>");
                else
                    $ul.append("<li>"+ this.cfg.values[i] +"</li>");
            }

            this.btn = this.boundingBox.find(".w-select-btn");
            this.valuesBar = this.boundingBox.find(".w-select-values");
            this.lis = this.boundingBox.find("li");
            if(this.cfg.parent != null)
                this.cfg.parent.append(this.boundingBox);
        },
        bindUI : function(){
            var that = this;
            this.on("show", function(){
                if(that.cfg.enable == false)
                    return;
                $(".w-select-btn").removeClass("w-select-btn-showing");
                $(".w-select-values").slideUp(100);
                that.btn.addClass("w-select-btn-showing");
                that.valuesBar.slideDown(400);
            });
            this.on("close", function(){
                that.btn.removeClass("w-select-btn-showing");
                that.valuesBar.slideUp(400);
            });
            this.on("selected", function(data){
                if(that.cfg.onSelect != null){
                    that.cfg.onSelect(data.sel, data.value);
                }
                //that.fire("close");
            });

            this.boundingBox.click(function(event){
                if(that.btn.hasClass("w-select-btn-showing"))
                {
                    that.fire("close");
                }
                else
                {
                    that.fire("show");
                }
                event.cancelBubble = true;
                event.stopPropagation();
            });

            this.lis.click(function(){
                var i = that.lis.index(this);
                that.setSelectIndex(i, true);
            });
        },
        syncUI : function(){
            //初始化设置选中项
            this.setSelectIndex(this.cfg.defaultSel, this.cfg.firstCallBack);
            this.setEnable(this.cfg.enable);
            if(this.cfg.css != null)
                this.boundingBox.css(this.cfg.css);
            if(this.cfg.width != null)
                this.boundingBox.find(".w-select").css("width", this.cfg.width);
        },
        destructor : function(){
        },
        //设置选中项
        setSelectIndex : function(index, isCallBack){
            isCallBack = isCallBack || false;

            this._select = index;
            var value = this.cfg.values[index];
            if(this.cfg.key != null)
                value = value[this.cfg.key];
            this.boundingBox.find(".w-select-value").empty().append( value );
            if(isCallBack)
            {
                this.fire("selected", { sel:index, value:this.cfg.values[index] });
            }
        },
        setSelectIndexByValue : function(value, isCallBack){
            for(var i in this.cfg.values){
                var thisValue = this.cfg.values[i];
                if(this.cfg.key != null)
                    thisValue = thisValue[this.cfg.key];
                if(value == thisValue)
                {
                    this.setSelectIndex(i, isCallBack);
                    break;
                }
            }
        },
        setEnable : function(type){
            this.cfg.enable = type;
            if(type == true){
                this.boundingBox.removeClass("w-select-box-unable");
            }
            else{
                this.boundingBox.addClass("w-select-box-unable");
            }
        },
        setSelectValues : function(_values, _defaultSel, _iscallback){
            var defaultSel = _defaultSel || 0;
            var iscallback = _iscallback || true;
            var $ul = this.boundingBox.find("ul");
            $ul.empty();
            this.cfg.values = _values;
            for(var i=0; i<this.cfg.values.length; i++)
            {
                if(this.cfg.key != null)
                    $ul.append("<li>"+ this.cfg.values[i][this.cfg.key] +"</li>");
                else
                    $ul.append("<li>"+ this.cfg.values[i] +"</li>");
            }
            this.setSelectIndex(defaultSel, iscallback);
        },
        //获取当前选中的内容
        getSelectValue : function(){
            var value = this.cfg.values[this._select];
            if(this.cfg.key != null)
                value = value[this.cfg.key];
            return {
                sel : this._select,
                value : value
            }
        }
    });
    if(typeof Select._init == 'undefined')
    {
        Select._init = true;
        $('body').on('click', function(){
            $(".w-select-btn").removeClass("w-select-btn-showing");
            $(".w-select-values").slideUp(100);
        });
    }
    return {
        Select : Select
    }
});
```