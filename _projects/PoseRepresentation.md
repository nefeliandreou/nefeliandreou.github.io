---
layout: default2
title: Pose Representations for Deep Skeletal Animation
description: 
img: assets/img/3_project.jpg
importance: 1
category: other
---
<html lang="en-US">
  <head>
    <meta charset="UTF-8">
<style>

.button {
  color: purple;
  padding: 15px 32px;
  text-align: center;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  border-radius: 10px;
}



.highlight {
  background-color: #f6f6f6;
  color:black;
}

 
</style>


<!-- Setup Google Analytics -->



<!-- You can set your favicon here -->
<!-- link rel="shortcut icon" type="image/x-icon" href="/text2mesh/favicon.ico" -->

<!-- end custom head snippets -->

  </head>
  <body>

    <header class="page-header" role="banner">
      <h1 class="project-name"><center><b>Pose Representations for Deep Skeletal Animation</b></center></h1>
      
      <br>
       <h3><center><b>SCA 2022</b></center></h3>
       <br>
      <h5 class="project-tagline"><center><a href="https://nefeliandreou.github.io">Nefeli Andreou</a><sup>1</sup> &nbsp; 
      <a href="http://andreasaristidou.com">Andreas Aristidou</a>
      <sup>1,2</sup> &nbsp; <a href="https://www.cs.ucy.ac.cy/~yiorgos/">Yiorgos Chrysanthou</a><sup>1,2</sup> </center> </h5>
      <p class="project-affiliation"><center><sup>1</sup><a href="https://www.ucy.ac.cy/en/">University Of Cyprus</a> &nbsp; 
      <sup>2</sup><a href="https://www.cyens.org.cy/en-gb/">CYENS Centre of Excellence</a> &nbsp; </center></p>

      
    </header>

    <main id="content" class="main-content" role="main">
      <center>
<figure style="display:inline-block;margin:0;padding:0"><img src="/assets/img/dq/DQ_21.gif" alt="teaser" width="100%" /><figcaption style="text-align:center"></figcaption></figure>
<p><em>
A fundamental component of motion modeling with deep learning is the pose parameterization. A suitable parameterization is one
that holistically encodes the rotational and positional components. The dual quaternion formulation proposed in this work can encode these
two components enabling a rich encoding that implicitly preserves the nuances and subtle variations in the motion of different characters
</em></p>
<a href="https://arxiv.org/abs/2111.13907" class="button button">Paper</a>
<a href="https://www.youtube.com/watch?v=bZKc_8s-XIk" class="button button">Video</a>
<a href="https://github.com/nefeliandreou/PoseRepresentation" class="button button">Code</a>
</center>

<hr />

<h2 id="abstract">Abstract</h2>
<p>Data-driven skeletal animation relies on the existence of a suitable learning scheme, which can capture the rich context of motion. 
However, commonly used motion representations often fail to accurately encode the full articulation of motion, or present
artifacts. In this work, we address the fundamental problem of finding a robust pose representation for motion, suitable for deep
skeletal animation, one that can better constrain poses and faithfully capture nuances correlated with skeletal characteristics.
Our representation is based on dual quaternions, the mathematical abstractions with well-defined operations, which simultaneously 
encode rotational and positional orientation, enabling a rich encoding, centered around the root. We demonstrate that
our representation overcomes common motion artifacts, and assess its performance compared to other popular representations.
We conduct an ablation study to evaluate the impact of various losses that can be incorporated during learning. Leveraging
the fact that our representation implicitly encodes skeletal motion attributes, we train a network on a dataset comprising of
skeletons with different proportions, without the need to retarget them first to a universal skeleton, which causes subtle motion
elements to be missed. Qualitative results demonstrate the usefulness of the parameterization in skeleton-specific synthesis.
 </p>
 
<h2 id="overview">Method</h2>
A dual quaternion \( \mathbf{\overline {q}} \)  can be represented as an ensemble of ordinary quaternions \( \mathbf{q}_r \)  and \( \mathbf{q}_d \) , in the form \( \mathbf{q}_r +  \mathbf{q}_d\varepsilon \) , where  \( \varepsilon \)  is the dual unit, satisfying the relation \( \varepsilon^2= 0 \) . The first quaternion describes the rotation. The second quaternion, \( \mathbf{q}_d \)  encodes translational information.
<br />
<br />
We establish the following notation: <br />
<div style="padding-left: 30px;">

\[ \begin{align}
\( \mathbf{q} \) & quaternion 
\( \mathbf{\overline{q}} \) & dual quaternion
\end{align} \]

<!-- \( \mathbf{q} \) quaternion <br />
\( \mathbf{\overline{q}} \) dual quaternion    <br />
\( \mathbf{\hat q} \) unit quaternion <br />
\( \mathbf{\overline{\hat q}} \) unit dual quaternion <br />
\( \mathbf{q^*}\)  quaternion conjugate <br />
\( \mathbf{\overline{q}^*} \) dual quaternion conjugate  <br /> -->
</div>
<br />
Dual quaternions allow for convenient mappings from and to other representations which are currently used in the literature (quaternions, rotation matrices, <a href="https://openaccess.thecvf.com/content_CVPR_2019/papers/Zhou_On_the_Continuity_of_Rotation_Representations_in_Neural_Networks_CVPR_2019_paper.pdf">ortho6D [Zhou et al., 2018]</a>), allowing for effortless integration into current architectures. They also have well established mathematical properties such as addition and multiplication. 
<br />
<br />
Due to the mathematical properties of dual quaternions we can extract the rotational and positional components. We adopt a current coordinate system, so that the extracted positions correspond to the joint positions with respect to the root joint. We model the root displacement as a separate component. We can express the current transformation of the <em>root</em> joint using <em>local homogeneous</em> coordinates of the form:
\[ \begin{equation}M_{curr,root} = 
\begin{bmatrix}
r_{11} &r_{12}&r_{13}&0\\
r_{21} &r_{22}&r_{23}&0\\
r_{31} &r_{32}&r_{33}&0\\
0&0&0&1
\end{bmatrix} 
\end{equation} \].
<br />
Then following the tree hierarchy and using the <em>local homogeneous</em> coordinates of each joint \( j \), 

\[ \begin{equation}M_{loc,j} = 
  \begin{bmatrix}
r_{11}  & r_{12} & r_{13} & \text{offset}_x\\
r_{21}  & r_{22} & r_{23} & \text{offset}_y \\
r_{31}  & r_{32} & r_{33} & \text{offset}_z\\
0       & 0      & 0      & 1
\end{bmatrix} ,
\end{equation}  \] 

where the \( \text{offset} \) is taken from the hierarchy of the bvh file,  we can compute the <em>current homogeneous</em> representation for each joint using

\[ \begin{equation}
M_{curr,j} = M_{curr,(j-1)} \times M_{loc,j}
\end{equation} \]

Obtaining the local rotations w.r.t. each joint's parent in the tree architecture, as well as the offsets is straightforward when animation files are used. Intuitively, we can recover the <em>local transformation</em> of joint \( j \) using the inverse procedure:
\[ \begin{equation}
M_{loc,j} = M^{-1}_{curr,(j-1)}M_{curr,j}
\label{eq: curr2loc}
\end{equation} \]

With our formulation we take into consideration the hierarchical rig, and overcome problems such as the error accumulation (see Figure below).
<center>
<figure style="display:inline-block;margin:0;padding:0"><img src="/assets/img/dq/rigexp.PNG" alt="Error Accumulation" width="50%" /><figcaption style="text-align:center"></figcaption></figure>
 </center>
 
 
We design dedicated losses that leverage the representation, such as the positional loss and offset loss. We assess the contribution of each loss through an ablation study. 
For the positional information, we extract the translation from the current dual quaternions, while for the offset loss we extract the translation from the local dual quaternions.
We extract the translation component using
\[\begin{aligned}
2 \mathbf{q}_d \mathbf{\hat{q}}_r^*
\end{aligned}\]
where \( \mathbf{\hat{q}}_r^* \) denotes the conjugate quaternion. 

We also make sure to normalize the predicted dual quaternions, as only unit dual quaternions represent valid rotations. We do so using
\[ \begin{align}
   \hat{\mathbf{\overline{q}}} &= \frac{\overline{\mathbf{q}}}{||\overline{\mathbf{q}}||} =   \frac{\mathbf{q}_r}{||\mathbf{q}_r||} + \varepsilon \bigg[ \frac{\mathbf{q}_d}{||\mathbf{q}_r||} -  \frac{\mathbf{q}_r}{||\mathbf{q}_r||}  \frac{<\mathbf{q}_r , \mathbf{q}_d>}{||\mathbf{q}_r||^2} \bigg] \nonumber \\
   &= \mathbf{\hat{q}}_r +  \varepsilon \bigg[ \frac{\mathbf{q}_d}{||\mathbf{q}_r||} - \mathbf{\hat{q}}_r  \frac{<\mathbf{q}_r , \mathbf{q}_d>}{||\mathbf{q}_r||^2} \bigg]
\end{align} \]

 
We experiment with two recurrent neural networks, the <a href="https://github.com/papagina/Auto_Conditioned_RNN_motion">acRNN</a> and <a href="https://github.com/facebookresearch/QuaterNet" >QuaterNet</a> and compare with the following representations: quaternions, quaternions with joint positions, quaternions with forward kinematic (FK) loss, ortho6D and ortho6D with positions.


 <h2 id="overview">Overview Video & Results</h2>
 <center>
 <iframe width="560" height="315" src="https://www.youtube.com/embed/bZKc_8s-XIk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
 </center>

<h2 id="citation">Citation</h2>

<div class="language-plaintext highlighter"><div class="highlight"><pre class="highlight"><code>
@article {Andreou:2022,
	journal = {Computer Graphics Forum},
	title = {Pose Representations for Deep Skeletal Animation},
	author = {Andreou, Nefeli and Aristidou, Andreas and Chrysanthou, Yiorgos},
	year = {2022},
	publisher = {The Eurographics Association and John Wiley & Sons Ltd.},
	ISSN = {1467-8659},
	DOI = {10.1111/cgf.14632}}

</code></pre></div></div>


 
    </main>
  </body>
</html>