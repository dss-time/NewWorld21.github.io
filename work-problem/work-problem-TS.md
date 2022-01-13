```react
const image = new Image();
    image.src = src;
    const imgWindow = window.open(src);
    imgWindow!.document.write(image.outerHTML);
```

<!--属性或参数中使用 ?. ：表示该属性或参数为可选属性。-->

<!--属性或参数后使用!. 表示强制解析（告诉ts这里有值），变量后使用!.表示类型判断排除null和undefined。-->



报错

![image-20220113140419986](/Users/ep/Library/Application Support/typora-user-images/image-20220113140419986.png)

解决

![image-20220113140625349](/Users/ep/Library/Application Support/typora-user-images/image-20220113140625349.png)

```react
import React, { useState } from 'react';
import { Upload } from 'antd';
import ImgCrop from 'antd-img-crop';
import { UploadFileStatus } from 'antd/lib/upload/interface';

interface uploadProps {
  fileList: string;
  uid: string;
  name: string;
  status: string;
  url: string;
}
const UploadD: React.FC<uploadProps> = () => {
  const [fileList, setFileList] = useState([
    {
      uid: '-1',
      name: 'image.png',
      status: 'done' as UploadFileStatus,
      url: 'https://zos.alipayobjects.com/rmsportal/jkjgkEfvpUPVyRjUImniVslZfWPnJuuZ.png',
    },
  ]);

  const onChange = ({ fileList: newFileList }) => {
    setFileList(newFileList);
  };

  const onPreview = async (file) => {
    let src = file.url;
    if (!src) {
      src = await new Promise((resolve) => {
        const reader = new FileReader();
        reader.readAsDataURL(file.originFileObj);
        reader.onload = () => resolve(reader.result);
      });
    }
    const image = new Image();
    image.src = src;
    const imgWindow = window.open(src);
    imgWindow!.document.write(image.outerHTML);
  };
  return (
    <ImgCrop rotate>
      <Upload
        action="https://www.mocky.io/v2/5cc8019d300000980a055e76"
        listType="picture-card"
        fileList={fileList}
        onChange={onChange}
        onPreview={onPreview}
      >
        {fileList.length < 5 && '+ Upload'}
      </Upload>
    </ImgCrop>
  );
};

export default UploadD;


//因为分配string给UploadFileStatus | undefined. UploadFileStatus可以是其中之一error | success | done | uploading | removed。
//ts类型判断
```

