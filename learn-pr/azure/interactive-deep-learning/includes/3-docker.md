## Docker

![Docker logo](../media/3-image1.PNG)

Docker enables developers to easily pack, ship, and run any  application as a lightweight, portable, self-sufficient container, which can run virtually anywhere. If the DSVM base image comes with the most popular deep learning frameworks pre-installed, why is there a need for containerization clients such as Docker?

Often when attempting to run deep learning tasks developers find themselves facing dependency nightmares, such as: 

- Having to build custom packages - Deep learning researchers tend to think less about production when they publish code to Git Hub. If they can get a package working on their own development environment, they often just assume that others will be able to do so as well.
- GPU driver versioning - CUDA is a parallel computing platform and application programming interface (API) developed by Nvidia. It allows software developers and software engineers to use a CUDA-enabled graphics processing unit (GPU) for general-purpose processing. Certain versions of Tensorflow will not work with versions of CUDA above 9.1 but other frameworks such as PyTorch seem to perform better with later versions of CUDA.

To get around these issues and to increase the usability of code, you can use Docker or its GPU variant Nvidia-Docker to manage and run deep learning projects. 

<!--Quiz 
What is CUDA? 
What versioning issues do deep learning engineers deal with? -->