---

title: uvflow shader

date: 2023-04-26 14:00:00

categories: shader

description: chatgpt

tags: shader

---

下面是一个简单的Unity shader示例，可以将UV沿着X轴正方向移动：

```
Shader "Custom/UVMove" {
    Properties {
        _MainTex ("Texture", 2D) = "white" {}
        _Speed ("Speed", Range(0, 10)) = 1
    }
    SubShader {
        Tags { "RenderType"="Opaque" }
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
            float _Speed;

            v2f vert (appdata v) {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.uv = v.uv;
                return o;
            }

            fixed4 frag (v2f i) : SV_Target {
                float2 uv = i.uv;
                uv.x += _Time.y * _Speed;
                fixed4 col = tex2D(_MainTex, uv);
                return col;
            }
            ENDCG
        }
    }
    FallBack "Diffuse"
}
```

这个shader有两个参数，一个是需要移动的纹理，另一个是移动速度。在顶点着色器中，我们将UV值传递给片段着色器，然后在片段着色器中使用_Time.y（Unity自带的变量，表示从游戏开始的时间）来计算偏移量，实现沿着X轴正方向移动的效果。

使用这个shader，只需要将其添加到一个材质上，然后将材质应用到一个3D模型上即可。需要注意的是，这个shader只支持单个纹理，如果需要移动多个纹理，需要进行一些修改。