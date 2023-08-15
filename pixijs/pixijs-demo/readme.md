## Application 参数说明

PixiJS 中的Application是管理Pixi应用程序的主类,它的构造函数有以下常用参数:

- width, height:应用程序的宽高尺寸。

- view: 应用程序将渲染到的HTMLCanvasElement或者WebGLRenderingContext。

- backgroundColor: 应用程序的背景色。

- resolution: 像素分辨率,默认是设备分辨率。

- autoDensity: 是否自动处理像素比,默认是true。

- antialias: 是否开启抗锯齿,默认是false。

- transparent: 是否开启透明背景,默认是false。 

- preserveDrawingBuffer: 是否保留drawing buffer数据,默认false。

- clearBeforeRender: 是否在渲染前清除画布,默认true。 

- roundPixels: 是否四舍五入非整数像素数,默认false。

- forceCanvas: 是否强制使用Canvas渲染,默认false。

- sharedTicker: 是否使用共享的ticker,默认false。

- sharedLoader: 是否使用共享的资源加载器,默认false。

Application还有一些其他不常用的参数,这些是最主要的配置参数,可以根据需要进行设置。

```html
<!DOCTYPE html>
<body>
</body>
<style>
    body,
    html {
        margin: 0;
        padding: 0;
        height: 100vh;
        overflow: hidden;

    }
</style>
<script src="https://pixijs.download/release/pixi.js"></script>

<script>
    const { Application } = window.PIXI;
    console.log(Application)

    const app = new Application({
        // 应用程序宽
        width: window.innerWidth,
        // 应用程序高
        height: window.innerHeight,
        // 背景颜色
        backgroundColor: 0x6495ed,
        // 分辨率参数
        resolution: window.devicePixelRatio || 2,
        // 是否自动处理像素比,默认是true
        autoDensity: true
    });
    document.body.appendChild(app.view);
</script>
```

如果全屏游戏的话就这样配置就可以了

```javascript
const app = new Application({
  view: canvas,
  resizeTo: window,
  autoDensity: true,
});
```

通过 `resizeTo` 属性指定应用画布跟随网页窗口尺寸，还可以在用户屏幕旋转、调整窗口尺寸后由 PixiJS 自动调整画布尺寸，以适配用户设备的最新画面状态。

——不过页面内的成员坐标和尺寸并不会按新旧尺寸的比例进行调整更新，毕竟实际游戏场景的成员数可能相当多，而且不同成员的定位适配策略通常并不相同，还是需要在检测到对应 `resize` 事件后自行调整。



## Sprite 精灵

PIXI.Sprite 是 PixiJS 中最常用的显示对象,用于在场景中显示图片或动画。它提供了一些常用的API:

1. texture：Sprite显示的纹理,可以更换精灵的外观。

2. width/height：获取或设置精灵的大小。

3. position：设置或获取精灵的位置坐标。

4. scale：对精灵进行缩放,如sprite.scale.set(2) 放大一倍。

5. rotation：旋转精灵,参数为旋转角度。

6. anchor：设置精灵的锚点,会影响旋转和缩放的中心点。

7. alpha：控制精灵的透明度。

8. tint：对精灵应用颜色填充。

   ```javascript
   // 以实现随机设置精灵的着色效果:
   const sprite = new PIXI.Sprite(texture);
   // 生成一个随机整数,作为sprite的tint颜色值 
   const color = Math.random() * 0xFFFFFF;
   sprite.tint = color;
   ```

9. interactive：开启精灵的交互功能。

10. buttonMode：开启按钮交互模式。

11. on()：绑定交互事件监听器,如点击、触碰等。

这些是Sprite最常用的一些基础API,可以通过它们控制精灵在场景中的各种显示效果和行为。

```html
<!DOCTYPE html>

<body>
</body>
<style>
    body,
    html {
        margin: 0;
        padding: 0;
        height: 100vh;
        overflow: hidden;

    }
</style>
<script src="https://pixijs.download/release/pixi.js"></script>

<script>
    const { Application } = window.PIXI;

    const app = new Application({
        resizeTo: window,
        autoDensity: true,
        // 背景颜色
        backgroundColor: 0x6495ed,

    });
    document.body.appendChild(app.view);
    // 添加纹理 
    const texture = PIXI.Texture.from('./example/assets/bunny.png');
    // 使用纹理生成精灵
    const sprite = new PIXI.Sprite(texture);
    console.dir(sprite)
    // 设置精灵在中间位置
    sprite.position.set(window.innerWidth / 2, window.innerHeight / 2)
    // 放大5倍
    sprite.scale.set(5)

    /**
     * 在 PixiJS 中,精灵Sprite的anchor属性用于设置精灵的锚点,它会影响精灵的旋转、缩放等转换的中心点。
        anchor的使用方法如下:
        anchor的默认值是(0,0),表示精灵的左上角。
        anchor的值范围是0到1,按精灵宽高的百分比设置。
    */
    // 设置到精灵中心点 0 
    sprite.anchor.set(0.5, 0.5);
    // 设置精灵透明度
    sprite.alpha = 0.5
    // 旋转精灵
    // 旋转90度
    // sprite.rotation = Math.PI / 2;
    // 第二种旋转
    // sprite.angle = 45
    // 开启交互
    sprite.interactive = true;
    sprite.on('click', function (e) {
        console.log('鼠标点击了')
        sprite.alpha = sprite.alpha === 1 ? 0.5 : 1
    })
    app.ticker.add((delta) => {
        // rotate the container!
        // use delta to create frame-independent transform
        sprite.rotation -= 0.01 * delta;
    })
    app.stage.addChild(sprite)

</script>
```

## Container

这里是PixiJS容器常用API的简要总结:

- PIXI.Container(): 创建一个新的容器。

- container.addChild(child): 向容器添加一个子对象。

- container.removeChild(child): 从容器中删除一个子对象。 

- container.removeChildren(): 删除容器中的所有子对象。

- container.getChildAt(index): 获取容器中指定索引位置的子对象。

- container.getChildByName(name): 通过名称获取子对象。

- container.sortChildren(): 对容器中的子对象进行排序。

- container.children: 容器的子对象数组。

- child.parent: 子对象所属的容器。

- child.transform: 子对象的变换(位置、缩放、旋转等)。

- container.x/y: 容器的位置。

- container.scale/pivot: 容器的缩放和旋转锚点。

- container.hitArea: 容器的点击区域。

- container.mask: 容器的遮罩。

- container.filters: 容器的滤镜效果数组。

- container.interactive: 容器的交互设置。

这些是PixiJS容器最常用的API,可以方便地对显示对象进行层级管理和转换。

```html
<!DOCTYPE html>

<body>
</body>
<style>
    body,
    html {
        margin: 0;
        padding: 0;
        height: 100vh;
        overflow: hidden;

    }
</style>
<script src="https://pixijs.download/release/pixi.js"></script>

<script>
    const { Application } = window.PIXI;

    const app = new Application({
        resizeTo: window,
        autoDensity: true,
        // 背景颜色
        backgroundColor: 0x6495ed,

    });
    document.body.appendChild(app.view);

    const container = new PIXI.Container()
    app.stage.addChild(container);


    // 添加纹理 
    const texture = PIXI.Texture.from('./example/assets/bunny.png');
    // 使用纹理生成精灵
    const sprite = new PIXI.Sprite(texture);

    // Create a 5x5 grid of bunnies
    for (let i = 0; i < 25; i++) {
        const bunny = new PIXI.Sprite(texture);
        bunny.anchor.set(1);
        bunny.x = (i % 5) * 40;
        bunny.y = Math.floor(i / 5) * 40;
        container.addChild(bunny);
    }

    // 移动container的位置 - 移动了后发现靠右偏下
    // container.position.set(window.innerWidth / 2, window.innerHeight / 2)
    container.x = app.screen.width / 2;
    container.y = app.screen.height / 2;

    // 设置到容器中心
    container.pivot.x = container.width / 2;
    container.pivot.y = container.height / 2;

    app.ticker.add((delta) => {
        container.rotation -= 0.01 * delta;
    })
</script>
```

PixiJS 中的 direction、turningSpeed 和 speed 这几个属性都用于控制精灵的移动和旋转,下面我通过一些代码案例详细介绍它们的用法:

1. direction

direction 控制精灵的渲染方向,它有两个值:

- PIXI.DIRECTION.CLOCKWISE:顺时针渲染(默认值)
- PIXI.DIRECTION.COUNTER_CLOCKWISE:逆时针渲染

示例:

```js
const sprite = new PIXI.Sprite(texture); 

// 默认顺时针
sprite.direction = PIXI.DIRECTION.CLOCKWISE;

// 修改为逆时针  
sprite.direction = PIXI.DIRECTION.COUNTER_CLOCKWISE; 

// 随机
 sprite.direction = Math.random() * Math.PI * 2;
```

这可以用来实现精灵的镜像翻转效果。

2. turningSpeed 

turningSpeed 控制精灵的自动旋转速度。

示例:

```js
sprite.turningSpeed = Math.PI; // 旋转速度为 π 弧度/秒

function update() {
  sprite.rotation += sprite.turningSpeed * deltaTime; 
} 

// 随机
sprite.turningSpeed = Math.random() - 0.8;
```

这会让精灵以稳定的速度自动旋转。

3. speed

speed 控制精灵的移动速度。

示例:

```js
sprite.speed = 5; // 移动速度为 5 像素/秒

function update() {
  sprite.x += sprite.speed * deltaTime;
}  

// 随机
dude.speed = 2 + Math.random() * 2;
```

这会让精灵以稳定的速度自动移动。

综上,这几个属性都可以实现平滑的移动/旋转动画,是PixiJS中非常重要的动画属性。

## 参考链接

[Container - PixiJS Examples](https://pixijs.io/examples/#/demos-basic/container.js)

[PixiJS API Documentation](https://pixijs.download/release/docs/index.html)