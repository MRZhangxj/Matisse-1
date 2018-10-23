![Image](/image/banner.png)

# 加强版已发布 该库后期不再更新维护， 使用方式与当前库一致， 具体使用参考[Matisse-kotlin地址](https://github.com/NFLeo/Matisse-Kotlin)

## Matisse
本项目在原项目上修改（2018/5月版本）
*主要修改内容为：*
```
1. 优化相册选择。
2. 优化单选策略。
3. 添加圆形与方形裁剪。  单图模式（maxSelectable == 1）才支持裁剪
4. 图片选择后压缩，不失真条件下高比率压缩。

* 注：裁剪成功后只返回裁剪后图片的绝对路径，不返回Uri，需自行转换

具体调用查看 SelectionCreator.java

关于打包报错问题：

使用：
1. 复制matisse依赖包
2. 为适配7.0，,项目manifest的privider标签下 paths文件中添加
    <external-path  name="my_images" path="Pictures"/>


主项目gradle中android目录下添加

    lintOptions{
        disable 'MissingTranslation'
    }

```

2018-05-08
修改内容：
    同步添加选中回调、选中原图功能

2018-02-07
修改内容：
    处理打包报错问题，删除多余语言支持

2017-11-29
修改内容：
    同步更新（ 11 月23 日前） 官方版本优化内容

2017-10-19 
修改内容：
    裁剪框尺寸与屏幕尺寸关联，修复小手机裁剪框出界
	
2017-10-12 
修改内容：
    图片选择成功后压缩，只对图片进行压缩，多张压缩只需循环执行
	

2017-10-11 
修改内容：
    解决选择视频时裁剪崩溃
	

2017-7-26 
修改内容：
    修复方形裁剪无法拍照
	
[![Build Status](https://travis-ci.org/zhihu/Matisse.svg)](https://travis-ci.org/zhihu/Matisse)  
Matisse is a well-designed local image and video selector for Android. You can  
- Use it in Activity or Fragment
- Select images including JPEG, PNG, GIF and videos including MPEG, MP4 
- Apply different themes, including two built-in themes and custom themes
- Different image loaders
- Define custom filter rules
- More to find out yourself

| Zhihu Style                    | Dracula Style                     | Preview                          |
|:------------------------------:|:---------------------------------:|:--------------------------------:|
|![](image/screenshot_zhihu.png) | ![](image/screenshot_dracula.png) | ![](image/screenshot_preview.png)|

| Circle Crop                    | Square Crop                       |
|:------------------------------:|:---------------------------------:|
|![](image/screenshot_circlecrop.png) | ![](image/screenshot_squarecrop.png) |

Check out [Matisse releases](https://github.com/zhihu/Matisse/releases) to see more unstable versions.
And add extra rule:
```pro
-dontwarn com.bumptech.glide.**
```

## How do I use Matisse?
#### Permission
The library requires two permissions:
- `android.permission.READ_EXTERNAL_STORAGE`
- `android.permission.WRITE_EXTERNAL_STORAGE`

So if you are targeting Android 6.0+, you need to handle runtime permission request before next step.

#### Simple usage snippet
------
Start `MatisseActivity` from current `Activity` or `Fragment`:

```java
Matisse.from(MainActivity.this)
        .choose(MimeType.allOf())
        .countable(true)
        .maxSelectable(9)
        .addFilter(new GifSizeFilter(320, 320, 5 * Filter.K * Filter.K))
        .gridExpectedSize(getResources().getDimensionPixelSize(R.dimen.grid_expected_size))
        .restrictOrientation(ActivityInfo.SCREEN_ORIENTATION_UNSPECIFIED)
        .thumbnailScale(0.85f)
        .imageEngine(new GlideEngine())
        .forResult(REQUEST_CODE_CHOOSE);

Matisse.from(SampleActivity.this)
        .choose(MimeType.ofAll(), false)      // 展示所有类型文件（图片 视频 gif）
        .capture(true)                        // 可拍照
        .countable(true)                      // 记录文件选择顺序
        .captureStrategy(new CaptureStrategy(true, "cache path"))
        .maxSelectable(1)                     // 最多选择一张
        .isCrop(true)                         // 开启裁剪
        .cropOutPutX(400)                     // 设置裁剪后保存图片的宽高
        .cropOutPutY(400)                     // 设置裁剪后保存图片的宽高
        .cropStyle(CropImageView.Style.RECTANGLE)   // 方形裁剪CIRCLE为圆形裁剪
        .isCropSaveRectangle(true)                  // 裁剪后保存方形（只对圆形裁剪有效）
        .addFilter(new GifSizeFilter(320, 320, 5 * Filter.K * Filter.K))  // 筛选数据源可选大小限制
        .gridExpectedSize(getResources().getDimensionPixelSize(R.dimen.grid_expected_size))
        .restrictOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT)
        .thumbnailScale(0.8f)
        .imageEngine(new GlideEngine())
        .forResult(REQUEST_CODE_CHOOSE);
```