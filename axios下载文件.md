# axios下载文件

## 场景说明

写vue项目用的axios库，在处理下载文件的时候，发现后端返回的是文件流，无法直接下载，只能用a标签处理，但是对于有鉴权的下载，a标签显然是不合适的了。

## 使用方法

### (1)   axios请求下载

请求下载的时候设置responseType为blob，例如：

```javascript
export function apiDownloadFiles(fielId) {
  return axios({
    url: `/document/${fielId}`,
    method: 'get',
    responseType: 'blob'
  })
}

```

这样请求就会走axios拦截器里配置的方法，你就可以给下载添加请求头token了

### (2)  将文件流处理成文件

```javascript
apiDownloadFiles(file.id).then(res => {
  if (res.data.type === "application/json") {
    this.$message({
      type: "error",
      message: "下载失败，文件不存在或权限不足"
    });
  } else {
    let blob = new Blob([res.data]);
    if (window.navigator.msSaveOrOpenBlob) {
      navigator.msSaveBlob(blob, file.fileName);
    } else {
      let link = document.createElement("a");
      let evt = document.createEvent("HTMLEvents");
      evt.initEvent("click", false, false);
      link.href = URL.createObjectURL(blob); 
      link.download = file.fileName;
      link.style.display = "none";
      document.body.appendChild(link);
      link.click();
      window.URL.revokeObjectURL(link.href);
    }
  }
});
```

根据实际情况修改, 

file.fileName : 生成文件的名字,如果后台没返回可以直接定义名字

### (3)  没太多限制,直接下载

模拟a标签点击，成功下载文件。 如果没有太多参数的下载文件，可以直接下载，这种没有token，比较适合用cookies和后端做认证的。

```javascript
const link = document.createElement('a')
const evt = document.createEvent('HTMLEvents')
evt.initEvent('click', false, false)
link.href = `${process.env.BASE_API}/template/download/${this.fileId}`
link.target = '_blank'
link.style.display = 'none'
document.body.appendChild(link)
link.click()
window.URL.revokeObjectURL(link.href)

```

## 以上总结来自于

https://juejin.im/post/5bc6e25d5188255c5a0d2adf