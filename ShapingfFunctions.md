#define  delta = 0.1
#ifdef GL_ES
 
precision mediump float;
#endif

// 1.0 GETTING STARTED WITH GLSL:
//Illustration of shaping function for
// 1) Linear, Ploynomial, Exponential
// 2) Trignometric
// 3) SmoothStep
// 4) Mathematical

uniform vec2 resolution;
uniform vec2 mouse;
uniform float time;

float DELTA = 0.007;


// Plot a line on Y using a value between 0.0-1.0
float plot(vec2 st, float pct){
  return  smoothstep( pct - DELTA, pct, st.y) -
          smoothstep( pct, pct + DELTA, st.y);
}

void main() {
	
    	 // pixel coordinates	
   	 vec2 st = gl_FragCoord.xy/resolution;
	
	// Linear interpolation of y = x
	float y =  st.x;
	
   	// Polynomial interpolation	
	 y = pow(st.x,5.0);
	
	//Exponential interpolation 
	 y = exp(st.x);
	
	// Smooth interpolation between 0.1 and 0.9
	 y = smoothstep(0.1,0.9,st.x);

	// Step will return 0.0 unless the value is over 0.5,
         // in that case it will return 1.0
	 y = step(0.5,st.x);
	
	 //combine smooth step
	 y = smoothstep(0.2,0.5,st.x) - smoothstep(0.5,0.8,st.x);

	
	// Trignomatric interpolation
          y =  sin(time);
	  y =  sin(time)*st.x;
	  y =  sin(time+st.x);
	  y =  sin(time*st.x);	
	  y =  abs(sin(time+st.x));
	  y =  fract(sin(time+st.x));
	  y =  floor(sin(time+st.x))+ceil(sin(time+st.x));
	
	 // Mathematical operators
	float x = st.x;
	 y = mod(x,0.5); // return x modulo of 0.5
         y = fract(x); // return only the fraction part of a number
         y = ceil(x);  // nearest integer that is greater than or equal to x
         y = floor(x); // nearest integer less than or equal to x
         y = sign(x);  // extract the sign of x
         y = abs(x);   // return the absolute value of x
         y = clamp(x,0.0,1.0); // constrain x to lie between 0.0 and 1.0
         y = min(0.0,x);   // return the lesser of x and 0.0
         y = max(0.0,x);   // return the greater of x and 0.0 
	
	vec3 color = vec3(y);

  	 // Plot a line
   	   float pct = plot(st,y);
   	   color = (1.0-pct) * color + pct*vec3(0.0, 1.0, 0.0);

    gl_FragColor = vec4(color,1.0);
}
