    #define  delta = 0.1
    #define PI 3.141592653589793
    #define HALF_PI 1.5707963267948966
    #ifdef GL_ES
    precision mediump float;
    #endif

    // 3.0 MIXING COLORS with Easing functions

    uniform vec2 resolution;
    uniform vec2 mouse;
    uniform float time;

    float DELTA = 0.007;

    vec3 yellow, magenta, green;


    // Simple exmple of creating colors
    vec4 getColor(int index)
    {       
       yellow.rg = vec2(1.0);  // Assigning 1. to red and green channels
       yellow[2] = 0.0;   

       if(index == 0)
       {

      // Making Yellow
           // Assigning 0. to blue channel
       return vec4(yellow,1.0);
       }else if(index == 1)
       {

       // Making Magenta
        magenta = yellow.rbg;   // Assign the channels with green and blue swapped
              return vec4(magenta,1.0);
       }else
       {

              // Making Green
              green.rgb = yellow.bgb; // Assign the blue channel of Yellow (0) to red and blue channels
        return vec4(green,1.0);
       }
    }

    // Robert Penner's easing functions in GLSL
    // https://github.com/stackgl/glsl-easings
    float linear(float t) {
      return t;
    }

    //---------------------EXPONENTIAL---------------

    float exponentialIn(float t) {
      return t == 0.0 ? t : pow(2.0, 10.0 * (t - 1.0));
    }

    float exponentialOut(float t) {
      return t == 1.0 ? t : 1.0 - pow(2.0, -10.0 * t);
    }

    float exponentialInOut(float t) {
      return t == 0.0 || t == 1.0
        ? t
        : t < 0.5
          ? +0.5 * pow(2.0, (20.0 * t) - 10.0)
          : -0.5 * pow(2.0, 10.0 - (t * 20.0)) + 1.0;
    }

    //---------------------SIN---------------
    float sineIn(float t) {
      return sin((t - 1.0) * HALF_PI) + 1.0;
    }

    float sineOut(float t) {
      return sin(t * HALF_PI);
    }

    float sineInOut(float t) {
      return -0.5 * (cos(PI * t) - 1.0);
    }

    //---------------------QINTIC---------------

    float qinticIn(float t) {
      return pow(t, 5.0);
    }

    float qinticOut(float t) {
      return 1.0 - (pow(t - 1.0, 5.0));
    }

    float qinticInOut(float t) {
      return t < 0.5
        ? +16.0 * pow(t, 5.0)
        : -0.5 * pow(2.0 * t - 2.0, 5.0) + 1.0;
    }


    //---------------------QUARTIC---------------
    float quarticIn(float t) {
      return pow(t, 4.0);
    }

    float quarticOut(float t) {
      return pow(t - 1.0, 3.0) * (1.0 - t) + 1.0;
    }

    float quarticInOut(float t) {
      return t < 0.5
        ? +8.0 * pow(t, 4.0)
        : -8.0 * pow(t - 1.0, 4.0) + 1.0;
    }

    //-----------------QUADRATIC---------------

    float quadraticInOut(float t) {
      float p = 2.0 * t * t;
      return t < 0.5 ? p : -p + (4.0 * t) - 1.0;
    }

    float quadraticIn(float t) {
      return t * t;
    }

    float quadraticOut(float t) {
      return -t * (t - 2.0);
    }

    //-----------------CUBIC---------------
    float cubicIn(float t) {
      return t * t * t;
    }

    float cubicOut(float t) {
      float f = t - 1.0;
      return f * f * f + 1.0;
    }

    float cubicInOut(float t) {
      return t < 0.5
        ? 4.0 * t * t * t
        : 0.5 * pow(2.0 * t - 2.0, 3.0) + 1.0;
    }

    //-------------ELASTIC--------------

    float elasticIn(float t) {
      return sin(13.0 * t * HALF_PI) * pow(2.0, 10.0 * (t - 1.0));
    }

    float elasticOut(float t) {
      return sin(-13.0 * (t + 1.0) * HALF_PI) * pow(2.0, -10.0 * t) + 1.0;
    }

    float elasticInOut(float t) {
      return t < 0.5
        ? 0.5 * sin(+13.0 * HALF_PI * 2.0 * t) * pow(2.0, 10.0 * (2.0 * t - 1.0))
        : 0.5 * sin(-13.0 * HALF_PI * ((2.0 * t - 1.0) + 1.0)) * pow(2.0, -10.0 * (2.0 * t - 1.0)) + 1.0;
    }

    //-------------CIRCULAR--------------

    float circularIn(float t) {
      return 1.0 - sqrt(1.0 - t * t);
    }

    float circularOut(float t) {
      return sqrt((2.0 - t) * t);
    }

    float circularInOut(float t) {
      return t < 0.5
        ? 0.5 * (1.0 - sqrt(1.0 - 4.0 * t * t))
        : 0.5 * (sqrt((3.0 - 2.0 * t) * (2.0 * t - 1.0)) + 1.0);
    }

    //-------------BOUNCE--------------

    float bounceOut(float t) {
      const float a = 4.0 / 11.0;
      const float b = 8.0 / 11.0;
      const float c = 9.0 / 10.0;

      const float ca = 4356.0 / 361.0;
      const float cb = 35442.0 / 1805.0;
      const float cc = 16061.0 / 1805.0;

      float t2 = t * t;

      return t < a
        ? 7.5625 * t2
        : t < b
          ? 9.075 * t2 - 9.9 * t + 3.4
          : t < c
            ? ca * t2 - cb * t + cc
            : 10.8 * t * t - 20.52 * t + 10.72;
    }

    float bounceIn(float t) {
      return 1.0 - bounceOut(1.0 - t);
    }

    float bounceInOut(float t) {
      return t < 0.5
        ? 0.5 * (1.0 - bounceOut(1.0 - t * 2.0))
        : 0.5 * bounceOut(t * 2.0 - 1.0) + 0.5;
    }


    //-------------BACK--------------

    float backIn(float t) {
      return pow(t, 3.0) - t * sin(t * PI);
    }

    float backOut(float t) {
      float f = 1.0 - t;
      return 1.0 - (pow(f, 3.0) - f * sin(f * PI));
    }

    float backInOut(float t) {
      float f = t < 0.5
        ? 2.0 * t
        : 1.0 - (2.0 * t - 1.0);

      float g = pow(f, 3.0) - f * sin(f * PI);

      return t < 0.5
        ? 0.5 * g
        : 0.5 * (1.0 - g) + 0.5;
    }

    void main() {

           // pixel coordinates	
         vec2 st = gl_FragCoord.xy/resolution;

      // colors to mix
             vec3 colorA = vec3(0.149,0.141,0.912);
             vec3 colorB = vec3(1.000,0.833,0.224);

       //mix percentage 
       float pct = abs(sin(time));

       // mix percentage using ease
       //float t = time*0.5;
       // t = abs(sin(time));
             // pct = qinticInOut( abs(fract(t)*2.0-1.0) );

            gl_FragColor = vec4(vec3(mix(colorA, colorB, pct)),1.0);

          // gl_FragColor =getColor(0);
    }
