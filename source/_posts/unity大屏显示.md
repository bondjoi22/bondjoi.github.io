---

title: unity 大屏显示

date: 2023-04-27 14:00:00

categories: unity

description: unity大屏

tags: unity大屏

---

如果你需要将大屏分成左、中、右三个区域，可以按照以下步骤进行操作：

1. 在场景中创建一个UI画布(Canvas)。

2. 在Canvas下创建三个UI RawImage，分别用来显示左、中、右三个区域的内容。

3. 在Assets文件夹下创建一个C#脚本，用来控制大屏的内容。以下是一个简单的示例：

```
using UnityEngine;
using UnityEngine.UI;

public class DisplayController : MonoBehaviour
{
    public RawImage leftImage;
    public RawImage middleImage;
    public RawImage rightImage;

    void Start()
    {
        //加载左、中、右三个区域的图片
        Texture2D leftTex = Resources.Load<Texture2D>("leftImage");
        Texture2D middleTex = Resources.Load<Texture2D>("middleImage");
        Texture2D rightTex = Resources.Load<Texture2D>("rightImage");

        //设置左、中、右三个区域的图片和宽高比
        leftImage.texture = leftTex;
        leftImage.aspectRatioFitter.aspectRatio = (float)leftTex.width / leftTex.height;

        middleImage.texture = middleTex;
        middleImage.aspectRatioFitter.aspectRatio = (float)middleTex.width / middleTex.height;

        rightImage.texture = rightTex;
        rightImage.aspectRatioFitter.aspectRatio = (float)rightTex.width / rightTex.height;

        //设置左、中、右三个区域的宽高和位置
        float screenWidth = Screen.width;
        float screenHeight = Screen.height;
        float imageWidth = screenWidth / 3;
        float imageHeight = screenHeight;
        float imageX = 0;
        float imageY = 0;

        leftImage.rectTransform.sizeDelta = new Vector2(imageWidth, imageHeight);
        leftImage.rectTransform.anchoredPosition = new Vector2(imageX, imageY);

        middleImage.rectTransform.sizeDelta = new Vector2(imageWidth, imageHeight);
        middleImage.rectTransform.anchoredPosition = new Vector2(imageX + imageWidth, imageY);

        rightImage.rectTransform.sizeDelta = new Vector2(imageWidth, imageHeight);
        rightImage.rectTransform.anchoredPosition = new Vector2(imageX + imageWidth * 2, imageY);
    }
}
```

4. 将脚本挂载到Canvas上，并将左、中、右三个RawImage分别赋值给脚本中的leftImage、middleImage、rightImage变量。

5. 将左、中、右三个区域的图片保存到Assets/Resources目录下，并分别命名为leftImage、middleImage、rightImage。

6. 运行程序，可以看到大屏被分成了左、中、右三个区域，并且分别显示了对应的图片内容。

需要注意的是，在实际应用中，你需要根据具体需求来设计大屏程序的功能和界面，并编写对应的代码。

是的，`RectTransform` 的 `sizeDelta` 属性用来表示UI元素的宽度和高度，它的值对应的是 Inspector 界面中 `Width` 和 `Height` 两个属性的值。通过设置 `sizeDelta` 属性，我们可以控制UI元素的大小。

需要注意的是，`RectTransform` 的 `sizeDelta` 属性表示的是UI元素的宽度和高度，而不是像素数或者其他单位。它的单位是与父物体的矩形区域大小相关的。例如，如果父物体矩形区域的宽度为100，高度为200，而子物体的 `sizeDelta` 属性值为（50，100），则子物体的宽度为50%的父物体宽度，高度为50%的父物体高度。

在使用 `RectTransform` 的时候，需要根据具体情况来设置 `sizeDelta` 的值，以达到所需的UI效果。

`RectTransform` 和 `Transform` 虽然都是用来控制物体的位置、旋转和缩放等属性，但是它们的实现方式是不同的，因此它们在一些方面是不同的。

具体来说，`Transform` 是用于控制3D物体的组件，它的坐标系是以世界坐标系为基础的。而 `RectTransform` 是用于控制UI物体的组件，它的坐标系是以父物体的矩形区域为基础的。

因此，`RectTransform` 的 `anchoredPosition` 属性实际上是表示UI元素的中心点相对于父物体矩形区域左下角的偏移量，而 `Transform` 的 `position` 属性则是表示物体中心点在世界坐标系中的位置。

虽然 `RectTransform` 和 `Transform` 的实现方式不同，但是它们都可以用来控制物体的位置、旋转和缩放等属性。在大多数情况下，我们只需要使用 `RectTransform` 或 `Transform` 中的一个即可满足我们的需求。但是在某些特殊情况下，可能需要同时使用 `RectTransform` 和 `Transform` 来实现更复杂的UI或3D效果。