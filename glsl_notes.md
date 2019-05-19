The final pixel color is assigned to the reserved global variable `gl_FragColor`
`vec4` a four dimensional vector of floating point precision
    * `vec4` has floating point precision and for that it expects to be assigned with floats. If you want to make good consistent code and not spend hours debugging white screens, get used to putting the point (.) in your floats.
there's also `vec3`, `vec2`. `float`, `int`, and bool
GLSL specs don’t guarantee that variables will be automatically casted

```c
vec4 color = vec4(vec3(1.0,0.0,1.0),1.0);
```

Each thread receives the same data from the CPU that it can read but cannot change. These inputs are called uniform and come in most of the supported types: float, vec2, vec3, vec4, mat2, mat3, mat4, sampler2D and samplerCube. Uniforms are defined with the corresponding type at the top of the shader right after assigning the default floating point precision.

```c
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;  // Canvas size (width,height)
uniform vec2 u_mouse;       // mouse position in screen pixels
uniform float u_time;       // Time in seconds since load
```

The names will vary from implementation to implementation but in this series of examples I’m always passing:
    * `u_time` (time in seconds since the shader started)
    * `u_resolution` (billboard size where the shader is being drawn)
    * `u_mouse` (mouse position inside the billboard in pixels).
I’m following the convention of putting u_ before the uniform name to be explicit about the nature of this variable but you will find all kinds of names for uniforms. For example ShaderToy.com uses the same uniforms but with the following names:

```c
uniform vec3 iResolution;   // viewport resolution (in pixels)
uniform vec4 iMouse;        // mouse pixel coords. xy: current, zw: click
uniform float iTime;        // shader playback time (in seconds)
```

```c
// use u_time - the number of seconds since the shader started running -
// together with a sine function to animate the transition of the amount of red
// in the billboard
void main() {
  // abs is a float function in GLSL apparently
  gl_FragColor = vec4(abs(sin(u_time)),0.0,0.0,1.0);
}
```

The GPU has hardware accelerated angle, trigonometric and exponential functions. Some of those functions are:
    * `sin`
    * `cos`
    * `tan`
    * `asin`
    * `acos`
    * `atan`
    * `pow`
    * `exp`
    * `log`
    * `sqrt`
    * `abs`
    * `sign`
    * `floor`
    * `ceil`
    * `fract`
    * `mod`
    * `min`
    * `max`
    * `clamp`

`vec4 gl_FragCoord` is a varying, not a uniform, which holds the screen coordinates of the pixel or screen fragment that the active thread is working on

```c
void main() {
  vec2 st = gl_FragCoord.xy/u_resolution;
  gl_FragColor = vec4(st.x,st.y,0.0,1.0);
}
```

`step` takes two parameters: the limit, and the value to check or pass.  Any value above the limit returns 1.0, any value below returns 0.0
`smoothstep` takes three parameters: a lower limit, an upper limit, and the value.  Creates a smooth interpolation between the lower and upper limits

```c
y = mod(x,0.999); // return x modulo of 0.5
y = fract(x); // return only the fraction part of a number
y = ceil(x);  // nearest integer that is greater than or equal to x
y = floor(x); // nearest integer less than or equal to x
y = sign(x);  // extract the sign of x
y = abs(x);   // return the absolute value of x
y = clamp(x,0.0,1.0); // constrain x to lie between 0.0 and 1.0
y = min(0.0,x);   // return the lesser of x and 0.0
y = max(0.0,x);   // return the greater of x and 0.0
```

```c
vec4 vector;
vector[0] = vector.r = vector.x = vector.s;
vector[1] = vector.g = vector.y = vector.t;
vector[2] = vector.b = vector.z = vector.p;
vector[3] = vector.a = vector.w = vector.q;
```
