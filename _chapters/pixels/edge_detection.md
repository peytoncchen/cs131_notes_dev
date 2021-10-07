# CS131 Lecture 4 
### Stanford University 
<div align="center"> Peyton Chen, Michelle Xing, Sotirios Papasotiriou, Edgar Roman, Jay Saleh, Alex Oseguera </div>

# 4.1 Overview of Edge Detection

### What are edges? 

We can think of edges as significant local changes in an image. Edges often occur on the boundary between two different regions in an image, such as a region with shadowing and a region without shadowing. Regardless of where they appear though, edges are an incredibely important part of image analysis. 

### How do we represent edges?

Edges are represented through normal lines as well as lines that are seen in drawings, paintings and pictures. With these edges, we are provided a way to communicate with each other as well as express ourselves culturally. Additionally, edges convey meaning about scenes and objects that allow us to navigate the world. 

![](https://drive.google.com/uc?export=view&id=1Cdl6pp5JbZWH0fgvibrfLZIu2SVpzsj0)

### Edges and Biology

Through many years of research, science has discovered that edges are a fundamental component of biology called visual systems. Much of what we understand about human vision comes from our understanding of how the brain interprets edges. 

One of the most famous studies is one done by Hubel and Wiesel in the 1950's and 1960's. In Hubel and Wiesel's study, they sought to answer the question, using electrophysiology: What is the visual processing system like in mammals? In order to solve this question they studied cat brains which were the most similar to human brains from the visual processing point of view. They placed eletrodes on the primary visual cortex of the cats and measured what stimuli made the the neurons respond excitedly. What they found was that there were many cells in the primary visual cortex but found that the most important ones were the simple cells which responded to oriented edges. In fact, they discovered that the cats' visual processing would start with those simple structures of the visual world, in other words, edges. 

Hubel and Wiesel Experiment Image:

![](https://drive.google.com/uc?export=view&id=1NUKoSfCG3bDQ411lHJCcd90OlzEeQrXo)

However, not all edges are equal in the human visual system. In a research study done by Biederman, researchers divided a drawing made only of edges into equal parts, so that each part was roughly 50 percent of the edges and the sum of its parts would make up the original drawing. Human subjects were then asked to name the objects from only one of the parts deleted complimentary images.
It turns out the ability to name was very dependent on which half was shown to them

![](https://drive.google.com/uc?export=view&id=1z6NPrve7bBWjAA-nMBMpGL0OxHBEnqwb)

In yet another study in neuroscience, people were placed in FMRI scanners and shown natural images of scenes. They would then capture the brain responses of the subjects from seeing these images and feed the response to an algorithm that would take the brain activity and guess what type of image the subjects were looking at. Then, they took those same images but instead generated the image with only edges (i.e removing color and other detail), showed them to the subjects and once again had the algorithm guess which image the subjects were looking at. They found that the algorithm was able to guess just as well using edge images as it was with fully detailed images, showing the brains affinity for edge detection.

![](https://drive.google.com/uc?export=view&id=1fxdeSfQHA2I4l9jLHhpiqedFq1VY7XED)

### What is the goal of edge detection? 

The goal of edge detection is to take images and identify sudden changes within them. The reason we want this is that, inuitively, much of semantic and shape information that we get from an image can be encoded in just the edges. Additionally, being able to represent image content with only edges might be more compact than using the raw pixels of an image which is desirable. 

### Why do we care about edges from the Computer Vision Perspective? 

With edges we can extract a lot of information about the world such as recognizing objects. Additionally, edges allow us to recover geometry and viewpoints in an image. 

### Origin of edges

Edges in images are normally produced by discontinuities found in images. Some of these discontinuities are:

Surface normal discontinuity:  
- This happens when two surfaces that have different normal vectors intersect. In this case we see two surfaces that are perpendicular to each other and therefore they form an edge

![](https://drive.google.com/uc?export=view&id=16bZP72dbWTdZfMTPZW5MrertFwhwbNzD)

Depth discontinuity:  
- In this case, one region in the image has a much different depth than the other 

![](https://drive.google.com/uc?export=view&id=1rNYkDGxbFSTpN9fACsIFHLOBnuyoE5-O)

Surface Color discontinuity:  
- In this case we see that some color of the surface is different from other colors on the surface

![](https://drive.google.com/uc?export=view&id=1r4nzf_uMDuBS1stY1kv9BlVUWfuAV_uL)

Illumination Discontinuity:  
- In this case there is a shadow which is caused by differences in illumination. 

![](https://drive.google.com/uc?export=view&id=1Ma9HiAqjWcEgj2qDkP_2CaZfGdVxrqsq)
