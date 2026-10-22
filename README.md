# Numerical Differentiation with the FFT

In the previous finite difference assignment, you found a linear system
for approximating $u'(0)$ by making a differentiation formula exact on
trigonometric basis functions $e^{\mathrm{i} kx}$. For $N=2m+1$ points, the modes
were $k=-m,\ldots,m$, and the grid points were equally spaced by $2\pi/N$.
Solving that system gave differentiation weights.

Now we use the same basis to approximate the function itself. Given
samples $u(x_j)$, we seek coefficients $c_k$ such that our approximant $p_N(x)$ can be written as

$$

p_N(x)=\sum_{k=-m}^{m}c_k e^{\mathrm{i} kx},\text{ with }
\sum_{k=-m}^{m}c_k e^{\mathrm{i} kx_j}=u(x_j).

$$

This is a linear system $A\boldsymbol c=\boldsymbol u$ with entries
$A_{jk}=e^{\mathrm{i} kx_j}$. Solving this interpolation system gives the
coefficients of the Fourier modes and is called the
*Discrete Fourier Transform* (DFT). When the points are equally spaced, symmetry in the matrix makes this
transformation very fast. The algorithm is called the
*Fast Fourier Transform* (FFT). FFTs are widely used in signal
processing and communications, including internet and mobile devices.
They also have applications to ODEs and PDEs that we will investigate.

## Part 1. Exploring the Fourier coefficients

We will first consider approximating the function $\sin(4x)$.

1. Create a grid consisting of $N$ equally spaced points between $0$
   and $2\pi$, excluding $2\pi$. In MATLAB, this is

   ```matlab
   x = linspace(0,2*pi,N+1);
   x = x(1:end-1);
   ```

2. Sample $\sin(4x)$ on the grid, then use the Fourier transform
   (typically `fft`) to determine the Fourier-mode coefficients.
   MATLAB's `fft` returns $F=Nc$, with the coefficients stored in a
   particular order.

3. Plot the real and imaginary parts of the FFT output as functions
   of the array index. You should see two nonzero entries, up to roundoff.
   At which indices do they occur? Are they real or imaginary?

4. Explore what happens as you change the frequency of the sine wave.
   How does that change the nonzero indices? What happens if you change the
   amplitude? What happens if you use cosine instead of sine? Now try linear
   combinations of sines and cosines.

Write a short explanation, supported by your plots, of what changes as
you vary the frequencies and amplitudes. Explain which positive and
negative Fourier modes the array entries represent.

## Part 2. Computing derivatives

Now we will use the FFT to compute derivatives, starting with $\sin(4x)$.
Restrict yourself to odd $N=2m+1$. For even $N$, the positive and negative
Nyquist modes give the same sampled values, so differentiating that mode
requires special care. We avoid that issue here. If you are curious, try sampling the function $\sin(Nx/2)$ using the approach in part 1.

1. As before, create a grid of equally spaced points on $[0,2\pi)$,
   sample the function, and retrieve the Fourier coefficients using the FFT.

2. Consider the Fourier representation of the interpolant and its
   derivative:

   $$

   p_N(x)=\sum_{k=-m}^{m}c_k e^{\mathrm{i} kx},\qquad
   p_N'(x)=\sum_{k=-m}^{m}\mathrm{i} k c_k e^{\mathrm{i} kx}.

   $$

   To find a derivative, multiply each coefficient by $\mathrm{i} k$. Take into
   account the special order of the coefficients that you investigated in
   Part 1: array indices are not the same as signed frequencies. Explain briefly why the multiplier is $\mathrm{i} k$ on this
   interval.

3. Take the inverse transform, usually called `ifft`, and plot
   the result on the grid together with the analytical derivative of
   $\sin(4x)$. Is there any error in this method? Explain your answer,
   distinguishing exact arithmetic from floating-point computation.
   Check the size of any imaginary part before plotting the real part.

4. Now differentiate

   $$

   f(x)=\exp\!\bigl(\sin x+\cos(7\sin x)\bigr)

   $$

   both analytically and using the approach above. Plot both the analytical and your approximate derivatives on the same axes.

5. Compute the error as odd $N$ increases. Use the maximum error
   at the grid points,

   $$

   E_N=\max_{0\leq j<N}|p_N'(x_j)-f'(x_j)|.

   $$

   Create a convergence plot of $E_N$ against $N$ on a log-log scale.
   What happens to the error? Does it have a usual algebraic convergence
   rate—that is, is the plot approximately a straight line with a fixed
   slope while the error decreases?

6. About how many points does it take for the error to plateau?
   Compare the normalized Fourier spectra of the function and its
   derivative just before, near, and after this point. What happens to the
   sizes of the high-frequency coefficients? Which frequencies are resolved
   at each grid size, and how does differentiation affect them? Use your
   observations to explain the error plateau.

**Submit:** Your code, the short write-up from Part 1, and the plots
and explanations requested in Part 2. Show the analytical derivative and
explain your frequency ordering. As before, you are encouraged to use AI tools to help you with coding. However, you are responsible for correctness. Ensure you understand the code and the underlying mathematics before submitting.
