---

title: 边缘发光shader

date: 2023-04-26 14:00:00

categories: shader

description: chatgpt

tags: shader

---



边缘发光shader是一种常用的效果，可以让物体的边缘产生发光效果。下面是一个简单的边缘发光shader示例：

```
Shader "Custom/Outline" {
    Properties {
        _Color ("Outline Color", Color) = (1,0,0,1)
        _MainTex ("Base Texture", 2D) = "white" {}
        _OutlineWidth ("Outline Width", Range(0.0, 0.1)) = 0.01
    }
    SubShader {
        Tags {"Queue"="Transparent" "RenderType"="Opaque"}
        LOD 100
        
        Pass {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #include "UnityCG.cginc"

            struct appdata {
                float4 vertex : POSITION;
                float2 uv : TEXCOORD0;
            };

            struct v2f {
                float2 uv : TEXCOORD0;
                float4 vertex : SV_POSITION;
            };

            sampler2D _MainTex;
            float4 _Color;
            float _OutlineWidth;

            v2f vert (appdata v) {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = v.uv;
                return o;
            }

            fixed4 frag (v2f i) : SV_Target {
                // 计算法向量
                float3 worldNormal = UnityObjectToWorldNormal(tex2D(_MainTex, i.uv).xyz);
                float3 worldPos = mul(unity_ObjectToWorld, i.vertex).xyz;
                float rim = 1.0 - saturate(dot(worldNormal, normalize(_WorldSpaceCameraPos - worldPos)));

                // 给边缘加上发光
                fixed4 col = tex2D(_MainTex, i.uv);
                col.rgb = lerp(col.rgb, _Color.rgb, rim * _OutlineWidth);
                col.a = col.a * _Color.a;
                return col;
            }
            ENDCG
        }
    }
    FallBack "Diffuse"
}
```

这个shader的主要思路是计算每个像素的法向量，然后根据法向量和相机位置之间的夹角来计算出一个值，表示该像素距离边缘的远近程度，从而实现边缘发光的效果。具体实现如下：

1. 在顶点着色器中，我们将顶点的坐标和UV值传递到片段着色器中。

2. 在片段着色器中，我们首先计算法向量和世界坐标。

3. 然后，我们计算出相机位置和世界坐标之间的向量，并将其与法向量进行点积，得到夹角的余弦值。使用saturate函数将其限制在0到1之间，并取1减去这个值，得到距离边缘的远近程度。

4. 最后，我们使用lerp函数将原来的颜色和轮廓颜色进行插值，得到一个发光的效果。

使用这个shader，只需要将其添加到一个材质上，然后将材质应用到一个3D模型上即可。需要注意的是，由于该shader使用了透明队列，需要将渲染队列设置为透明，否则无法正常渲染。

  float rim = 1.0 - saturate(dot(worldNormal, normalize(_WorldSpaceCameraPos - worldPos)));

这段代码是在计算表面法线和相机方向之间的角度差，用于实现一些特殊的效果，如边缘发光和描边等。具体来说，代码中的

`dot(worldNormal, normalize(_WorldSpaceCameraPos - worldPos))`

表示表面法线和相机位置向量之间的点积，这个点积结果的取值范围在-1到1之间。因为点积的结果是两个向量之间的夹角的余弦值，所以我们使用

`saturate()`

函数将结果值限制在0到1之间。

_WorldSpaceCameraPos - worldPos

计算的是相机位置向量和当前像素位置向量之间的差值，即相机到当前像素的向量。由于这个向量是从当前像素位置指向相机位置的，因此使用`normalize()`函数将它转换为单位向量。

`worldNormal`表示当前像素的表面法线向量，它是指垂直于表面的向量。这个法线向量是由Unity自动计算得到的，它与表面的方向和倾斜度相关。在这段代码中，我们用这个法线向量和相机到像素的向量来计算它们之间的夹角余弦值。

最终，我们将这个余弦值减去1，得到一个从-1到0的范围。这个范围可以用于实现边缘发光或描边效果，例如，将这个值作为颜色插值因子，可以让边缘部分的颜色更加鲜明，从而让物体看起来更加立体和有轮廓感。