---
layout:    post
title:     Vue+Echarts实现动态网络拓扑图
subtitle: 
date:       2021-02-01 00:04:56
author:     terese
cover: /img/post-11.jpg
thumbnail: /img/post-11.jpg
catalog:   true
toc: true
category: 前端开发
tags:
    - 实践
typora-copy-images-to: upload
---

#### Vue+Echarts实现动态网络拓扑图
<!--more-->
![image-20210309211334067](https://i.loli.net/2021/03/16/mMqxOzAcyK6kaeg.jpg)

点击可出现各个主机信息详情，这里使用的是虚拟数据；

![image-20210314111922703](https://i.loli.net/2021/03/16/RWqsQu8XDJtFapA.jpg)

点击单个主机可以获取主机的信息

```
this.myChart.setOption(option)
    }
    },
  mounted () {
      this.drawChart()
    this.myChart.on('click', function (params) {
    // 弹窗打印数据的名称
    var description = ''
    var i
    var property
    // this.clickedIp = params.data.ip
    axios({
          method: 'get',
          url: 'http://127.0.0.1:7002/api/taobao/findall'
        }).then(resp => {
          this.nodesInfo = resp.data
          console.log(resp.data)
        }).catch(resp => {
            console.log(resp)
          console.log('请求失败：' + resp.status + ',' + resp.statusText)
      })
    if (params.dataType === 'node') {
        for (i in this.nodesInfo) {
        description += i + '=' +  this.nodesInfo[i] + '\n'
        }
        alert(description)
    } else if (params.dataType === 'edge') {
        for (i in this.nodesInfo) {
        property = this.nodesInfo
        description += i + ' = ' + property + '\n'
        }
        console.log(description)
        alert(description)
    }
      })
```

主机图像自定义，这里是各个主机图片存放地址，如果调试时出现dataindex未定义，可能是图片这里出问题，可以选择注释掉 symbol和symbolSize，使用symbol: 'circle'，先用默认的圆形调试出画面，再解决图像问题，写对路径就没有问题了。

```
 var node = {
        name: nodes[j].name,
        value: [x, y],
        symbolSize: 40,
        alarm: nodes[j].alarm,
        symbol: 'image://' + require('../../assets/' + nodes[j].img),
        //这里是各个主机图片存放地址
    //    symbol: 'circle',
       itemStyle: {
            normal: {
                color: '#12b5d0'
            }
        }
    }
```

主机与主机之间不通/通的情况，使用点和线之间的动态效果区分，结点之间通不通通过判断结点的state项，只要线其中一端主机出现state=0，线路就显示暂停传输，动态效果暂停。注意这种动态效果必须对点指定[x,y]坐标，所以需要固定位置。

```
 const getNode = nodes.find((nodes) => nodes.name === links[i].source)
  if (getNode.state === '1') {
          labelName = '数据传输中'
        } else {
          labelName = '暂停传输'
        }
if (getNode.state === '1') {
          link.lineStyle.normal.color = '#27b0fe'
          link.label.normal.color = '#27b0fe'
          // 组装动态移动的效果数据
          var lines = [{
             coord: dataMap.get(links[i].source)
    }, {
        coord: dataMap.get(links[i].target)
    }]
          charts.linesData.push(lines)
        }
```

总的来说，使用了Echart中的Gragh和Line。各个结点（即主机）展示内存使用率、硬盘使用率、CPU使用率、连通性等信息，通过对接API点击获取。

```
<template>
  <div class="tuopu-view">
    <el-card shadow="hover" :body-style="{ padding: '0 0 20px 0' }">
      <div class="tuopu-view-chart-wrapper">
        <div id="main2" style="width: 100%;height:400px;"></div>
      </div>
    </el-card>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      myChart: '',
    //   clickedIp: '',
      nodesInfo: ''
    }
  },
  methods: {
    drawChart () {
this.myChart = this.$echarts.init(document.getElementById('main2'))
var nodes = [{
        x: '1',
        y: '1',
        name: '派出所',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '1',
        y: '10',
        name: '派出所1',
        img: 'computer.png',
        state: '0'
    },
    {
        x: '3',
        y: '5',
        name: '应用服务器',
        img: 'server.png',
        state: '1'
    },
    {
        x: '5',
        y: '3',
        name: 'Redis',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '5',
        y: '6',
        name: '备份负载',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '5',
        y: '9',
        name: '主流通道',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '7',
        y: '3',
        name: '交换1',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '7',
        y: '6',
        name: '交换2',
        img: 'computer.png',
        state: '0'
    },
    {
        x: '7',
        y: '9',
        name: '交换3',
        img: 'computer.png',
        state: '0'
    },
    {
        x: '9',
        y: '3',
        name: '隔离1',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '9',
        y: '6',
        name: '隔离2',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '9',
        y: '9',
        name: '隔离3',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '11',
        y: '3',
        name: '交换4',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '11',
        y: '6',
        name: '交换5',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '11',
        y: '9',
        name: '交换6',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '13',
        y: '5',
        name: '网关',
        img: 'computer.png',
        state: '1'
    },
    {
        x: '15',
        y: '5',
        name: '应用服务器1',
        img: 'server.png',
        state: '1'
    },
    {
        x: '17',
        y: '5',
        name: '数据中心',
        img: 'computer.png',
        state: '1'
    }
]
var links = [{
        source: '应用服务器',
        target: '备份负载',
        name: '访问'
    },
      {
        source: '派出所1',
        target: '应用服务器',
        name: '访问'
    },
    {
        source: '应用服务器',
        target: 'Redis',
        name: '访问'
    },
    {
        source: '应用服务器',
        target: '备份负载',
        name: '访问'
    },
    {
        source: '应用服务器',
        target: '主流通道',
        name: '访问'
    },
    {
        source: '主流通道',
        target: '交换3',
        name: '访问'
    },
      {
        source: '派出所',
        target: '应用服务器',
        name: '访问'
    },
    {
        source: '备份负载',
        target: '交换2',
        name: '访问'
    },
    {
        source: 'Redis',
        target: '交换1',
        name: '访问'
    },
      {
        source: '交换3',
        target: '隔离3',
        name: '访问'
    },
    {
        source: '交换2',
        target: '隔离2',
        name: '访问'
    },
    {
        source: '交换1',
        target: '隔离1',
        name: '访问'
    },
      {
        source: '隔离3',
        target: '交换6',
        name: '访问'
    },
    {
        source: '隔离2',
        target: '交换5',
        name: '访问'
    },
    {
        source: '隔离1',
        target: '交换4',
        name: '访问'
    },
    {
        source: '交换6',
        target: '网关',
        name: '访问'
    },
    {
        source: '交换5',
        target: '网关',
        name: '访问'
    },
    {
        source: '交换4',
        target: '网关',
        name: '访问'
    },
      {
        source: '网关',
        target: '应用服务器1',
        name: '访问'
    },
    {
        source: '应用服务器1',
        target: '数据中心',
        name: '访问'
    }
]
var charts = {
    nodes: [],
    links: [],
    linesData: []
}
var dataMap = new Map()
for (var j = 0; j < nodes.length; j++) {
    var x = parseInt(nodes[j].x)
    var y = parseInt(nodes[j].y)
    var node = {
        name: nodes[j].name,
        value: [x, y],
        symbolSize: 40,
        alarm: nodes[j].alarm,
        symbol: 'image://' + require('../../assets/' + nodes[j].img),
        //这里是各个主机图片存放地址
    //    symbol: 'circle',
       itemStyle: {
            normal: {
                color: '#12b5d0'
            }
        }
    }
    console.log(node.symbol)
    dataMap.set(nodes[j].name, [x, y])
    charts.nodes.push(node)
}
var labelName = ''
for (var i = 0; i < links.length; i++) {
    const getNode = nodes.find((nodes) => nodes.name === links[i].source)
        console.log(getNode)
        // console.log(getNode.state)
        if (getNode.state === '1') {
          labelName = '数据传输中'
        } else {
          labelName = '暂停传输'
        }
    var link = {
        source: links[i].source,
        target: links[i].target,
        label: {
            normal: {
              show: true,
              formatter: labelName,
              color: '#ed46a2',
              fontSize: 12,
              fontWeight: 'normal'
            }
          },
          lineStyle: {
            normal: {
              color: '#ed46a2',
              width: 0.5
            }
          }
    }
    charts.links.push(link)
        if (getNode.state === '1') {
          link.lineStyle.normal.color = '#27b0fe'
          link.label.normal.color = '#27b0fe'
          // 组装动态移动的效果数据
          var lines = [{
             coord: dataMap.get(links[i].source)
    }, {
        coord: dataMap.get(links[i].target)
    }]
          charts.linesData.push(lines)
        }
}
const option = {
    title: {
        text: '网络拓扑图'
    },
    // tooltip: {
    //     trigger: 'item',
    //     formatter: '{b}'
    // },
     tooltip: {
        trigger: 'item',
        axisPointer: {
        type: 'shadow'
        },
        formatter: function(params) {
          // alert(JSON.stringify(params))
        //   console.log('JSON.stringify(params)')
        //   console.dir(params)
          var res = ''
          for (var i = 0; i < params.length; i++) {
            res += '<p>' + params[i].data.value + ':' + params[i].data.name + '</p>'
          }
          return res
        }
    },
    backgroundColor: '#F5F5F5',
    xAxis: {
        min: 0,
        max: 18,
        show: false,
        type: 'value'
    },
    yAxis: {
        min: 0,
        max: 12,
        show: false,
        type: 'value'
    },
    series: [{
        type: 'graph',
        layout: 'none',
        id: 'a',
        coordinateSystem: 'cartesian2d',
        edgeSymbol: ['', 'arrow'],
        // symbolSize: 50,
        label: {
            normal: {
                show: true,
                position: 'bottom',
                color: '#12b5d0'
            }
        },
        lineStyle: {
            normal: {
                width: 2,
                shadowColor: 'none',
                opacity: 1
                // curveness: 0.1
            }
        },
        xAxis: {
            min: 0,
            max: 12,
            show: false,
            type: 'value'
        },
        yAxis: {
            min: 0,
            max: 12,
            show: false,
            type: 'value'
        },
        // edgeSymbolSize: 8,
        draggable: true,
        data: charts.nodes,
        links: charts.links,
        z: 4,
        legendHoverLink: true,
        itemStyle: {
            normal: {
                label: {
                    show: true,
                    formatter: function(item) {
                        return item.data.name
                    }
                }
            }
        }
    }, {
        name: 'A',
        type: 'lines',
        coordinateSystem: 'cartesian2d',
        z: 4,
        effect: {
            show: true,
            trailLength: 0,
            symbol: 'arrow',
            color: '#12b5d0',
            symbolSize: 8
        },
        lineStyle: {
            normal: {
                curveness: 0
            }
        },
        data: charts.linesData
    }]
}
this.myChart.setOption(option)
    }
    },
  mounted () {
      this.drawChart()
    this.myChart.on('click', function (params) {
    // 弹窗打印数据的名称
    var description = ''
    var i
    var property
    // this.clickedIp = params.data.ip
    axios({
          method: 'get',
          url: 'http://127.0.0.1:7002/api/taobao/findall'// 这里是虚拟数据API
        }).then(resp => {
          this.nodesInfo = resp.data
          console.log(resp.data)
        }).catch(resp => {
            console.log(resp)
          console.log('请求失败：' + resp.status + ',' + resp.statusText)
      })
    if (params.dataType === 'node') {
        for (i in this.nodesInfo) {
        description += i + '=' +  this.nodesInfo[i] + '\n'
        }
        alert(description)
    } else if (params.dataType === 'edge') {
        for (i in this.nodesInfo) {
        property = this.nodesInfo
        description += i + ' = ' + property + '\n'
        }
        console.log(description)
        alert(description)
    }
      })
    }
}
</script>
<style lang="scss" scoped="scoped">
.tuopu-view {
  margin-top: 20px;
  .tuopu-view-chart-wrapper {
    display: flex;
    height: 500px;
  }
  .echarts {
    flex: 0 0 70%;
    width: 70%;
    height: 100%;
  }
}
</style>
```

