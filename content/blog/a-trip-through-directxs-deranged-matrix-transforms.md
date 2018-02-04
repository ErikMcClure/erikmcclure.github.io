+++
title = "A Trip Through DirectX's Deranged Matrix Transforms"
date = 2015-04-23T02:49:00Z
updated = 2015-04-23T02:49:07Z
draft = true
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

If you are a mathematician who is familiar with Matrix Algebra, there is something about the DirectX documentation that seems a bit... *odd*. And by odd, I mean it looks completely wrong:

{{<div style="margin-left:3em">}}<pre>D3DXMATRIX* D3DXMatrixMultiply(
  _Inout_  D3DXMATRIX *pOut,
  _In_     const D3DXMATRIX *pM1,
  _In_     const D3DXMATRIX *pM2
);</pre>

The result represents the transformation M1 followed by the transformation M2 (Out = M1 * M2).{{</div>}}
The function definition clearly states that D3DXMatrixMultiply performs the mathematical operation {{<code>}}M1 * M2{{</code>}}, but then states something that is impossible: that it represents the transformation M1 followed by M2. As a [trip to Wikipedia]() will tell you (and you can [confirm this on wolfram alpha](http://www.wolframalpha.com/input/?i=%7B%7B1%2C0%2C3%7D%2C%7B0%2C1%2C1%7D%2C%7B0%2C0%2C1%7D%7D*%7B%7B0%2C-1%2C0%7D%2C%7B1%2C0%2C0%7D%2C%7B0%2C0%2C1%7D%7D*%7B%7B1%7D%2C%7B0%7D%2C%7B1%7D%7D), too), multiplying the matrix M1 with M2 applies M2 to M1. The operations are applied in reverse order.



So, either the function is wrong, or the documentation is, right? **Wrong.** The documentation here is *correct*, it's just misleading. It's possible to get this kind of behavior if you *tranpose* all the matrices, because of this handy little property of matrix multiplication:

{{<math>}}(M N)^T = N^T M^T{{<math>}}

So, if you transpose both matrices and multiply them together, the result is equivalent to multiplying them in reverse order and transposing the result. This could therefore be fairly easily explained if DirectX either stored it's matrices in *column-major* order instead of *row-major*, or the multiplication function interpreted them as the opposite, which would effectively tranpose the matrices. However, it is clearly stated, multiple times, that [D3DXMATRIX is row-major](). We can confirm this by taking advantage of the fact that D3DXMatrix is really just an array of 16 floats, and {{<code>}}memcpy{{</code>}} our own row-major, canonical form matrix into a D3DXMATRIX object, then using D3DXMatrixMultiply. The result?

{{<pre>}}

{{</pre>}}
Huh. So D3DXMatrixMultiply is clearly doing a mathematically correct matrix multiplication, because we get the right answer when we use our own matrices. How can the documentation be right, then? When we multiply *our* matrices, we clearly are applying M2 to M1, not the other way around!

DirectX has a secret. It's a secret that, for some reason, it doesn't tell you about anywhere on any page that I could find on MSDN. All of the transformation functions in DirectX return *transposed versions of the canonical forms*:

{{<pre>}}

{{</pre>}}
***Oh.***




Why did Microsoft choose to do this, assuming they didn't do it just to make it harder to use OpenGL? The likely explanation is that it is much more intuitive to have M1*M2*M3 actually apply the operations in the order M1->M2->M3, which was then applied to a vector by multiplying the vector by the transform in question. Sadly, as a consequence of not mentioning that all of their transformations were actually transposed versions of the canonical affine transformations, this inadvertently confused the heck out of anyone who was coming from a Matrix Algebra course.
