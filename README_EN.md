# Yii2-image-cropper
Yii2's image crop widget

-----
[![Latest Stable Version](https://poser.pugx.org/coderfixlab/yii2-image-cropper/v)](//packagist.org/packages/coderfixlab/yii2-image-cropper) [![Total Downloads](https://poser.pugx.org/coderfixlab/yii2-image-cropper/downloads)](//packagist.org/packages/coderfixlab/yii2-image-cropper) [![Latest Unstable Version](https://poser.pugx.org/coderfixlab/yii2-image-cropper/v/unstable)](//packagist.org/packages/coderfixlab/yii2-image-cropper) [![License](https://poser.pugx.org/coderfixlab/yii2-image-cropper/license)](//packagist.org/packages/coderfixlab/yii2-image-cropper)

## install

Recommended to use composer to install

```shell
composer require coderfixlab/yii2-image-cropper
```

## Quick configuration 

### view

```php

    <?=  $form->field($model, 'image')->widget(\coderfixlab\cropper\Cropper::className(), [
        'label' => '选择图片',
        'imageUrl' => Yii::$app->tools->getImagesUrl($model->image), //preview
        'value'=>$model->image, //value
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
            'url'=>'/upload/upload-crop', //upload path
            'response'=>'res.url', //preview url
            'attachment'=>'res.attachment' //value 
        ],
        'jsOptions' => [
            'pos' => \yii\web\View::POS_END, // default is POS_END if not specified
          
            'onClick' => 'function(event){ 
                    
                }'
        ],
    ]); ?>

```

### Controller

 `uploadOptions` - `url`

Will simulate the file field of the form submission image

Reference processing code

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



## Screenshot


### Page load
![Page load](https://img-blog.csdnimg.cn/20200710135631425.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RpYW5kaWFueGl5dQ==,size_16,color_FFFFFF,t_70) 

### Crop 

![裁切图片](https://img-blog.csdnimg.cn/20200710135740746.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RpYW5kaWFueGl5dQ==,size_16,color_FFFFFF,t_70)

### Crop complete

![裁切完成](https://img-blog.csdnimg.cn/2020071013581885.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RpYW5kaWFueGl5dQ==,size_16,color_FFFFFF,t_70)


## TODO

 - Image upload status prompt
 - Perfect documentation

## Open source agreement

MIT License.