# antdesign-upload组件的使用以及文件存储展示

*在使用的过程中有一个小功能是，前端上传文件，后端存储在数据库中。再进行查询并展示，其中遇到了几个小点，在这里记录一下*

------

##### 一，upload组件的使用

upload组件上设置listType为"picture-card"，展示缩略图的形式，同时使用antd中的Image组件进行预览的功能。

```javascript
<Upload
    action="http://localhost:1023/beans/image/upload"
    listType="picture-card"
    fileList={fileList}
    onPreview={handlePreview}
    onChange={handleChange}
    onRemove={removeImage}
    >
        {uploadButton}
    </Upload>
    {previewImage && (
        <Image
        wrapperStyle={{ display: 'none' }}
    preview={{
             visible: previewOpen,
             onVisibleChange: (visible) => setPreviewOpen(visible),
                 afterOpenChange: (visible) => !visible && setPreviewImage(''),
    }}
    src={previewImage}
    />
        )}
```

使用中正常上传时可以预览缩略图以及完整的图的，在数据库中存储时，只存储了文件名和文件内容两个字段。查询出相应数据后，展示缩略图的话需要thumbUrl字段，预览的话需要preview字段。且Image组件预览时需要base64字符串类型的文件内容，记得处理。

```javascript
const modalList = res.data.map((item: { image: any; })=>{
  return { ...item,thumbUrl:`data:image/jpeg;base64,${item.image}`,preview:`data:image/jpeg;base64,${item.image}` };})
```

##### 二，上传完成后的回调函数

翻遍了upload组件的属性，没看到有成功后的回调函数，后面看到上传时候状态变化执行的都是onChange回调函数。

最初的写法如下，调试时发现只有uploading的状态

```JavaScript
   const handleChange = ({ file, fileList, event }: UploadChangeParam<UploadFile<any>>) => {
        if (file.status==='done') {
            queryList()
        }
    };
```

搜索后发现，需要在onChange的回调函数中执行对fileList进行set的方法，会执行到done的状态

```javascript
    const handleChange = ({ file, fileList, event }: UploadChangeParam<UploadFile<any>>) => {
        setFileList(fileList)
        if (file.status==='done') {
            queryList()
        }
    };
```

