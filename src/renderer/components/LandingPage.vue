<template>
    <!--<div id="wrapper">-->
    <!---->
    <!--</div>-->
    <el-row gutter="50" type="flex" justify="center" align="middle" class="is-fullscreen">
        <el-col :span="12">
            <el-row type="flex" justify="center" align="middle" class="child_padding">
                <el-button v-on:click="startPlayer">播放</el-button>
            </el-row>
            <el-row type="flex" justify="center" align="middle" class="child_padding">
                <el-button v-on:click="pausePlayer">暂停</el-button>
            </el-row>
            <el-row type="flex" justify="center" align="middle" class="child_padding">
                <el-button v-on:click="stopPlayer">停止</el-button>
            </el-row>
        </el-col>
        <el-col :span="12">
            <el-row type="flex" justify="center" align="middle" class="child_padding">
                <el-button v-on:click="jiaPlayer">音量+</el-button>
            </el-row>
            <el-row type="flex" justify="center" align="middle" class="child_padding">
                <el-button v-on:click="jianPlayer">音量-</el-button>
            </el-row>
            <el-row type="flex" justify="center" align="middle" class="child_padding">
                <el-button v-on:click="addPlayer">添加</el-button>
            </el-row>
        </el-col>
    </el-row>
</template>

<script>
  import SystemInformation from './LandingPage/SystemInformation'

  const net = require('net')
  export default {
    name: 'landing-page',
    data: function () {
      return {
        list: new Set(),
        server: null
      }
    },
    components: {SystemInformation},
    created: function () {
      var _this = this
      this.server = net.createServer(function (socket) {
        // 创建socket服务端
        console.log('connect: ' +
          socket.remoteAddress + ':' + socket.remotePort)
        socket.setEncoding('binary')
        _this.list.add(socket)
        // 接收到数据
        socket.on('data', function (data) {
          console.log('client send:' + data)
        })
        socket.write('Hello client!\r\n')
        // socket.pipe(socket);
        // 数据错误事件
        socket.on('error', function (exception) {
          console.log('socket error:' + exception)
          socket.end()
        })
        // 客户端关闭事件
        socket.on('close', function (data) {
          console.log('client closed!' + data)
          _this.list.delete(socket)
          // socket.remoteAddress + ' ' + socket.remotePort);
        })
      })
      this.server.listen(8080)
      // 服务器监听事件
      this.server.on('listening', function () {
        console.log('server listening:' + this.server.address().port)
      })
      // 服务器错误事件
      this.server.on('error', function (exception) {
        console.log('server error:' + exception)
      })
    },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      },
      startPlayer (event) {
        this.list.forEach(socket => {
          console.log('播放')
          socket.write('播放1')
        })
      },
      pausePlayer (event) {
        this.list.forEach(socket => {
          socket.write('暂停2')
        })
        console.log('暂停')
      },
      stopPlayer (event) {
        this.list.forEach(socket => {
          socket.write('停止3')
        })
        console.log('停止')
      },
      jiaPlayer (event) {
        this.list.forEach(socket => {
          socket.write('音量+4')
        })
        console.log('音量+')
      },
      jianPlayer (event) {
        this.list.forEach(socket => {
          socket.write('音量-5')
        })
        console.log('音量-')
      },
      addPlayer (event) {
        this.list.forEach(socket => {
          socket.write('添加6')
        })
        console.log('添加')
      }
    }
  }
</script>

<style>
    @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }

    body {
        font-family: 'Source Sans Pro', sans-serif;
    }

    #wrapper {
        background: radial-gradient(
                ellipse at top left,
                rgba(255, 255, 255, 1) 40%,
                rgba(229, 229, 229, .9) 100%
        );
        height: 100vh;
        padding: 60px 80px;
        width: 100vw;
    }

    #logo {
        height: auto;
        margin-bottom: 20px;
        width: 420px;
    }

    main {
        display: flex;
        justify-content: space-between;
    }

    main > div {
        flex-basis: 50%;
    }

    .child_padding {
        margin: 50px;
    }

    .is-fullscreen {
        height: 100%;
        width: 100%;
        top: 0;
        bottom: 0;
    }

    .left-side {
        display: flex;
        flex-direction: column;
    }

    .welcome {
        color: #555;
        font-size: 23px;
        margin-bottom: 10px;
    }

    .title {
        color: #2c3e50;
        font-size: 20px;
        font-weight: bold;
        margin-bottom: 6px;
    }

    .title.alt {
        font-size: 18px;
        margin-bottom: 10px;
    }

    .doc p {
        color: black;
        margin-bottom: 10px;
    }

    .doc button {
        font-size: .8em;
        cursor: pointer;
        outline: none;
        padding: 0.75em 2em;
        border-radius: 2em;
        display: inline-block;
        color: #fff;
        background-color: #4fc08d;
        transition: all 0.15s ease;
        box-sizing: border-box;
        border: 1px solid #4fc08d;
    }

    .doc button.alt {
        color: #42b983;
        background-color: transparent;
    }
</style>
