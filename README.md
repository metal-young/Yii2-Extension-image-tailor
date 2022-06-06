# Yii2-Extension-image-tailor
Yii2的图片裁切小部件

-----
[![Latest Stable Version](https://poser.pugx.org/coderfixlab/yii2-image-cropper/v)](//packagist.org/packages/coderfixlab/yii2-image-cropper) [![Total Downloads](https://poser.pugx.org/coderfixlab/yii2-image-cropper/downloads)](//packagist.org/packages/coderfixlab/yii2-image-cropper) [![Latest Unstable Version](https://poser.pugx.org/coderfixlab/yii2-image-cropper/v/unstable)](//packagist.org/packages/coderfixlab/yii2-image-cropper) [![License](https://poser.pugx.org/coderfixlab/yii2-image-cropper/license)](//packagist.org/packages/coderfixlab/yii2-image-cropper)
## 安装

推荐使用composer安装

```shell
composer require [coderfixlab/yii2-image-cropper](https://packagist.org/packages/metalyoung/yii2-extension-image-tailor)
```

## 快速配置 


### model

不要把`image`设置成file，看做文本即可，因为图片上传操作并没有经过业务的表，部件返回给input的实际上是个文本。

```php

    public function rules() {
        return [
            [['image'], 'string'],
        ];
    }

```

### view

```php

    <?=  $form->field($model, 'image')->widget(\coderfixlab\cropper\Cropper::className(), [
        'label' => '选择图片',
        'imageUrl' => Yii::$app->tools->getImagesUrl($model->image), //页面展示的图片路径
        'value'=>$model->image, //在input中作为表单值的图片路径
        'cropperOptions' => [
            //裁切尺寸
            'width' => 518, 
            'height' => 250, 
            //预览尺寸
            'preview' => [
                'width' => 518, 
                'height' => 250,
            ],
        ],
        'uploadOptions'=>[
            'url'=>'/upload/upload-crop', //图片上传路径
            'response'=>'res.url', //返回预览的完整图片地址
            'attachment'=>'res.attachment' //返回给input中作为表单值的图片路径
        ],
        'jsOptions' => [
            'pos' => \yii\web\View::POS_END, // default is POS_END if not specified
          
            'onClick' => 'function(event){ 
                    
                }'
        ],
    ]); ?>

```

### 上传控制器

图片上传到 `uploadOptions`的`url`地址中

会模拟表单提交image的文件域

参考处理代码

```php

    public function actionUploadCrop(){
        if (Yii::$app->request->isPost) {
            $file = $_FILES['image'];
            $md5 = md5_file($file['tmp_name']);
            $fileName = 'upload/'.date("Y/m").'/'.$md5;
            Yii::$app->get("qiniu")->upload($file['tmp_name'],$fileName);
        }
        $fullUrl = Yii::$app->get("qiniu")->getDomain().'/'.$fileName;
        return json_encode([
            'code'=>0,
            'msg'=>'上传成功',
            'url' => $fullUrl,
            'attachment'=>$fileName
        ]);
        exit();
    }


```

注意：必定返回的为在`uploadOptions`中定义的`url`和`attachment`，但是并不唯一，当修改视图配置后对应返回格式也要修改



## 截图


### 页面载入
![页面](https://img-blog.csdnimg.cn/20200710135631425.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RpYW5kaWFueGl5dQ==,size_16,color_FFFFFF,t_70) 

### 裁切图片

![裁切图片](https://img-blog.csdnimg.cn/20200710135740746.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RpYW5kaWFueGl5dQ==,size_16,color_FFFFFF,t_70)

### 裁切完成

![裁切完成](https://img-blog.csdnimg.cn/2020071013581885.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RpYW5kaWFueGl5dQ==,size_16,color_FFFFFF,t_70)


## TODO

 - 图片上传状态提示
 - 文档完善

## 开源协议

本项目基于 MIT License.
