---
title: "Generative Adversarial Networks For 3D Objects"
categories:
  - Blog
tags:
  - Machine Learning
  - 3D
  - GAN
# link: https://github.com
---

## Generative Adversarial Networks For 3D Objects

### 1. Overall Structure

GAN is a deep learning framework that can generate new objects following the same distribution as training data. It normally consists of a Generator(G) and a Discriminator(D). G generates new fake objects and D classifies the real objects from the repository and the fake objects that Generator generates. The intuition of this system is to make G stronger and D weaker, and the ultimate goal is to make D classify fake objects and real objects equally.

Normally, the inputs of G should be some latent codes, in other words, random noise. Then feed them into deep networks, the output should be the same size as training data.

To train the model, we should set the criteria and optimizers for both G and D.

#### a. Basic GAN model 

In the basic model, use a series of d-dimensional vectors randomly sampled by normal distribution as the prior latent code. The structure is as below(in red rectangles): 

<img src="../assests/images/gan/image-20220217030345961.png" alt="image-20220217110353374" style="zoom:50%;" />

​																					Figure 1 Basic structure of 3D point GAN model



But in this naive model, the network can not converge, and after 20 epochs, the inference output of the class chair is still random noise. see figure2.  It is because fully connected layers cannot maintain structural information and latent code is to simple. To get a better output, I add the sphere prior as the reference paper[^1] did. See section 1.b.

<img src="../assests/images/gan/image-20220217104301415.png" alt="image-20220217110353374" style="zoom:50%;" />

​																			Figure 2. Inference output of the basic model



The discriminator structure can be shown in figure 3, which remains the architecture of the discriminator in SP-GAN. It calculates the per-shape score and per-point score, which make the model more realistic and have more fine details.

<img src="../assests/images/gan/image-20220217110353374.png" alt="image-20220217110353374" style="zoom:30%;" />

​																				Figure 3. Discriminator structure of basic model

#### b. Advanced Model With Sphere Prior

As the input latent code plays important role in GAN, adding more variants to the model can make significant improvement.

After sphere prior added, it can be fed into a graph attention module, see figure 4, as proposed by DGCNN [^3].

<img src="../assests/images/gan/image-20220217114649132.png" alt="image-20220217114649132" style="zoom:35%;" />

After 24 epochs, D(G)  and D(real) gradually reach to 0.5, as real_acc and fake_acc in the sample output denotes:

```python
real_acc: 0.692311  fake_acc: 0.869046
D_x: 0.623101  D_G_z1: 0.299601  D_G_z2 0.765128
lr_g: 0.000100  lr_d: 0.000100
100%|██████████| 282/282 [08:28<00:00,  1.80s/it]
Save Path for G: log/23_Chair_G.pth
```

 After adding sphere prior, the inference output of the class chair, see figure 4, is more reasonable.

<img src="../assests/images/gan/image-20220217095934172.png" alt="image-20220217095934172" style="zoom:30%;" />

​															Figure 4. The generated result by new model of class chair



### 2. Visulization

Use ```matplotlib.pyplot``` to visualize the output point cloud, and use``` matplotlib.widgets.Button```  to add interactive functions:

- Click button ***Call Network***, and the generate model will be loaded.

<img src="../assests/images/gan/image-20220217111840718.png" alt="image-20220217111840718" style="zoom:30%;" />

​																				Figure 4. User interface of GAN visualization

- Click button ***Load Noise*** , will generate a sphere noise x and new latent code z.

- Click button ***Modify Noise*** , will generate a noew latend code z.

- Finally click button *** Dispaly***, will show the inference result(Figure 5):

  <img src="../assests/images/gan/image-20220217112910358.png" alt="image-20220217112910358" style="zoom:40%;" />

  ​																   Figure 5. Display after calling network and generating noise 



### 3. Code Implementation

I used Colab to write code and train the model.  ```PCGAN.py``` is the naive model without sphere prior, and ```PCGAN_F.py``` is the advanced model, both including training code, code to save/load model and visulization code.  The code is heavyly borrowed from [^1]and partially referenced from [^2].



### Reference

[^1]: R. Li, X. Li, K, Hui, C.-W. Fu, et al. SP-GAN: Sphere-Guided 3D Shape Generation and Manipulation. SIGGRAPH 2021.

[^2]: https://pytorch.org/tutorials/beginner/dcgan_faces_tutorial.html
[^3]: Yue Wang, Yongbin Sun, Ziwei Liu, Sanjay E. Sarma, Michael M. Bronstein, and Justin M. Solomon. 2019. Dynamic graph CNN for learning on point clouds. ACM Transactions on Graphics 38, 5 (2019), 146:1–146:12.

