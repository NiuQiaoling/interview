### h5页面包含meta标签

    - 基本meta

            <!--设置缩放-->
            <meta name="viewport" content="initial-scale=1.0,user-scalable=no,maximum-scale=1,width=device-width"/>
            <!--iOS中禁止将数字识别为电话号码-->
            <meta name="format-detection" content="telephone=no"/>

    
    - 搜索引擎meta

            搜索引擎索引方式：通常有如下几种取值：none，noindex，nofollow，all，index和follow。
            <meta name="robots" content="index,follow" />
            <!--
                all：文件将被检索，且页面上的链接可以被查询；
                none：文件将不被检索，且页面上的链接不可以被查询；
                index：文件将被检索；
                follow：页面上的链接可以被查询；
                noindex：文件将不被检索；
                nofollow：页面上的链接不可以被查询。
            -->
        
    - 页面缓存meta

            <!-- 清除缓存 -->
            <meta http-equiv="pragma" content="no-cache">
            <meta http-equiv="cache-control" content="no-cache">
            <meta http-equiv="expires" content="0">
        
### 常用css设置


    a,button,input,textarea { 
        -webkit-tap-highlight-color: rgba(0,0,0,0);
    }   
    /*或者*/
    * { -webkit-tap-highlight-color: rgba(0,0,0,0); }


    /*禁止长按链接与图片弹出菜单*/
    a,img { -webkit-touch-callout: none }    

    /*禁止ios和android用户选中文字*/
    html,body {-webkit-user-select:none; user-select: none; }

    /*改变输入框placeholder的颜色值*/
    ::-webkit-input-placeholder { /* WebKit browsers */
    color: #999; }
    :-moz-placeholder { /* Mozilla Firefox 4 to 18 */
    color: #999; }
    ::-moz-placeholder { /* Mozilla Firefox 19+ */
    color: #999; }
    :-ms-input-placeholder { /* Internet Explorer 10+ */
    color: #999; }
    input:focus::-webkit-input-placeholder{ color:#999; }

    /*android上去掉语音输入按钮*/
    input::-webkit-input-speech-button {display: none}

    /*input在iOS下禁用英文首字母默认大写*/
    <input autocapitalize="off" autocorrect="off" />

    // **消除transition闪屏**
    .css {
        -webkit-transform-style: preserve-3d;
        -webkit-backface-visibility: hidden;
        -webkit-perspective: 1000;
    }

### 屏幕旋转

js

    function orientInit(){
        var orientChk = document.documentElement.clientWidth > document.documentElement.clientHeight?'landscape':'portrait';
        if(orientChk =='lapdscape'){
            //横屏下需要执行代码
        }else{
            //竖屏下需要执行代码
        }
    }
    orientInit();
    window.addEventListener('onorientationchange' in window?'orientationchange':'resize', function(){
        setTimeout(orientInit, 100);
    },false)

css 

    /*竖屏时样式*/
    @media all and (orientation:portrait){   }
    /*横屏时样式*/
    @media all and (orientation:landscape){   }
