# @netless/oss-upload-manager

> alibaba cloud storage &amp; netless upload

[![NPM](https://img.shields.io/npm/v/@netless/oss-upload-manager.svg)](https://www.npmjs.com/package/@netless/oss-upload-manager) [![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

## 1. 说明

本项目技术选型为：`React` `Typescript `
打包工具为： `rollup`  

## 2. 安装

```bash
npm install --save @netless/oss-upload-manager

或者

yarn add @netless/oss-upload-manager
```


## 3. 接口说明

**自定义类型**

```typescript
export enum PPTProgressPhase {
    // 上传
    Uploading,
    // 转换
    Converting,
}

export enum PptKind {
    Dynamic = "dynamic",
    Static = "static",
}

// 由白板 SDK 提供
const pptConverter: PptConverter = whiteWebSdk.pptConverter(this.props.netlessToken);
```
| 初始化参数   | 说明              | 类型   | 默认值 |
| :----------- | :---------------- | :----- | :----: |
| ossClient | 阿里云 OSS 的对象 | any |   |
| room    | 白板房间对象        | Room |        |
| onProgress   | 进度监听       | (phase: PPTProgressPhase, percent: number) => void |        |

```typescript
 const uploadManager = new UploadManager(
   this.client,
   this.props.room,
   this.props.onProgress);
```

| 成员方法 | 说明  |
| :------- | :------------ |
| convertFile     | 上传并转换文件 |
| uploadImageFiles     | 上传图片，支持多张一起传 |

```typescript
// 上传文件
// 方法参数
rawFile: File,
pptConverter: PptConverter,
kind: PptKind,
target: {
  bucket: string,
  folder: string,
  prefix: string,
},

uploadManager.convertFile(
         event.file,
         pptConverter,
         PptKind.Static,
         {
             bucket: oss.bucket,
             folder: oss.folder,
             prefix: oss.prefix,
         })
 
 // 上传文件图片到正中央位置
 // 方法参数
 imageFiles: File[],
 x: number,
 y: number,
 
const {clientWidth, clientHeight} = this.props.whiteboardRef;
uploadManager.uploadImageFiles(
uploadFileArray,
clientWidth / 2,
clientHeight / 2)
```



## 4. 使用概览

```typescript
import Fetcher from "@netless/oss-upload-manager";

// 上传文件
private uploadStatic = (event: any) => {
    const uploadManager = new UploadManager(this.client, this.props.room, this.props.onProgress);
    const whiteWebSdk = new WhiteWebSdk();
    const pptConverter = whiteWebSdk.pptConverter(this.props.netlessToken);
    uploadManager.convertFile(
        event.file,
        pptConverter,
        PptKind.Static,
        {
            bucket: this.props.oss.bucket,
            folder: this.props.oss.folder,
            prefix: this.props.oss.prefix,
        }).catch(error => alert("upload file error" + error));
}

// 上传图片
private uploadImage = (event: any) => {
        const uploadFileArray: File[] = [];
        uploadFileArray.push(event.file);
        const uploadManager = new UploadManager(this.client, this.props.room, this.props.onProgress);
        const {clientWidth, clientHeight} = this.props.whiteboardRef;
        uploadManager.uploadImageFiles(uploadFileArray, clientWidth / 2, clientHeight / 2)
                     .catch(error => alert("upload file error" + error));
    }
```



## License

MIT © [alwaysmavs](https://github.com/alwaysmavs)
