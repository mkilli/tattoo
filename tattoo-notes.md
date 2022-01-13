#AI Tattoos#
##Tattoo Generator##

<img src="https://instagram.fykz1-2.fna.fbcdn.net/v/t51.2885-15/e15/269655146_467093708169107_2022479008308589667_n.jpg?_nc_ht=instagram.fykz1-2.fna.fbcdn.net&_nc_cat=111&_nc_ohc=S8dzYRTYh70AX_PYG0v&edm=AP_V10EBAAAA&ccb=7-4&oh=00_AT_V7ydyMRbbY0UTy3UyafRbJzROdUBKwMT-c_btSXzhUQ&oe=61E662D6&_nc_sid=4f375e" alt="drawing" width="200"/>
<img src="https://instagram.fykz1-2.fna.fbcdn.net/v/t51.2885-15/e15/263738950_692782252111024_6759077077435000632_n.jpg?_nc_ht=instagram.fykz1-2.fna.fbcdn.net&_nc_cat=104&_nc_ohc=u0Pld-UrBcsAX_R23c6&edm=AABBvjUBAAAA&ccb=7-4&oh=00_AT_3R4GCBm1YFdGjj_Zagwh1MMWJ9Jo6AGj3FvebOy-sgA&oe=61E6F77A&_nc_sid=83d603" alt="drawing" width="200"/>

AI Genreated Designs


<img src="https://instagram.fykz1-2.fna.fbcdn.net/v/t51.2885-15/sh0.08/e35/s640x640/264366027_3050566595199607_6673368094550173986_n.jpg?_nc_ht=instagram.fykz1-2.fna.fbcdn.net&_nc_cat=107&_nc_ohc=UpD5e6V4veMAX-A4tWf&tn=ptP6b2ZPEPe1hSsu&edm=AABBvjUBAAAA&ccb=7-4&oh=00_AT9yZi35XytAweiMEuqM9q66Ly9kbLrSAtv1mc0MJzYIDw&oe=61E69C46&_nc_sid=83d603" alt="drawing" height="200"/>

Left: AI generated Right: Real

###Overview##
Motivated by the NVIDIA's work on generating realistic high resolution images of faces, I was curious to see if [StytleGAN2-ADA](https://github.com/NVlabs/stylegan2-ada) could be used to generate novel new tattoo designs. 

As this is a personal project, I was focused on entirely on generating abstract designs for my personal interest, not general designs to satisfy a broad set of users or to generate realistic images.

###Dataset###
![reals](reals.jpg)
The training data consisted of approximately 10k high quality tattoo images. I removed all realism tattoos, those with overly complex backgrounds, and only retained images with tattoos on limbs.

The images come from about a dozen artists. Since there are number of images per artist is different, the data set is imbalanced.

I chose note to retain the artist label in the training, although this could be done to create a conditional model. This would allow you to make queries such as "create a tattoo in the style of XX artist.

###Preprocessing###
* Removed greyscale images.  
* Removed images smaller than 720x720.  
* Resized images to be 720x720. (square images only)  
* Created multiple copies of the dataset at different resolutions.


###Tattoo Exploration###
Once the model is trained, there are various techniques to explore and generate tattoo images.

Note - this works very well for abstract designs. However, if the user has a specific idea in mind, this would not be a good user experience. 

1. **Generate Random Tattoos**. A series of images can be created using random "seed" values to generate tattoos. Since there is no control over the type of tattoo that would be generated, the UX is not great for the average user.
![fakes](fakes000688.jpg)  

	*Possible Improvements*  
	i) One possible improvement is to generate a whole bunch of tattoo designs in advance, then manually curate them.
	
	ii) Alternatively, they could be categorized algorithmically using a clustering algorithm. For example, similar images would be grouped together. 
	
2. **Explore Similar Tattoos**. Once a tattoo is identified, the user could explore similar tattoos. In this case, the user would submit the "key" (i.i. random seed value) to the tattoo they like. Then, the algorithm would slowly morph the tattoo. The user could then stop the morphing on the design they like.
<p align="center">
  <img src="evolve.gif" />
</p>


3. **Style transfer** Once a tattoo is identified, the user could change the style (e.g. color palette) to match another tattoo. Right now there is not a lot of control. The user can basically say, "take this basic tattoo design, and draw it to look similar to the style of this other tattoo".

4. **Random exploration**. Pick a starting tattoo and randomly morph it the design, style, color etc. 

<p align="center">
  <img src="random.gif" />
</p>

5. **Transform an image into a tattoo**. User would upload an image and the algorithm would then "search" for an AI generated tattoo that closely matches it. Again, this works well if the user is looking for a highly abstract design. (though it could be improved upon).

###Improvements###
**Increase control over the style/design**. If the tattoo images were labelled (e.g. by artist name, by style, by subject matter), it would be possible to have a higher degree of control of the tattoo being generated. For instance, the user could specify the artist and/or style of the design to be generated. 

This would require a very high quality and large dataset. The best approach would be to limit this to the most requested tattoos (e.g. skulls) and styles (e.g. neo trad) and start from there.  


##Get Inked##
###Overview###
The objective of this usecase is to alter an image of a person by placing a tattoo on the subject's body.

This would likely demand a higher degree of control. The tattoo design shouldn't be altered and should be placed on a realistic location on the body -- perhaps even defined by the user.

###Approach###

There are a variety of techniques possible. Regarding the positioning of the tattoo, there are 3 general approaches:  

a. *Learned*. In this case, the user does not specify the location of the tattoo. The model would have to implicitly "learn" where to place the tattoo in the image. The feasibility is unclear, but should be possible if there is sufficient data.  

b. *2-step*. Again, the user does not specify the location of the tattoo. However, the location of the tattoo would be explicitly learned during training. The first model takes in an image of a person (without a tattoo) and outputs the target location of the tattoo (i.e. segmentation). The second model inserts the tattoo onto that location.  

c. *Conditional*. Here, the user would define the location of the tattoo. Again, there a variety of weighs. The simplest would be to train a model where the location of the tattoo is labeled (say by an x) in the images. After training, the user would have to mark an x on the desired location of the tattoo, in addition to specify the tattoo design.   

###Discussion###
With the right approach, this use case seem possible but could be challenging. If this was restricted to specific set of tattoo designs, the development would be easier. The challenge is to create a model that can take a *new* tattoo design and insert it in the image.

In principle, the model should not have to learn to create the tattoo image. It just needs to learn the transformations (resizing, croping, rotation, etc), to fit the design into the target location on the image. 

###Dataset###
Each example in the training data consists of i) the original image of a person (without tattoo), ii) the specific tattoo design, and augmented image of a person (with the tattoo).   

###References###
 
Related work seem to suggest this is possible. The developers of [SkinDeep](https://github.com/vijishmadhavan/SkinDeep) were able to create a model that could *remove* tattoos on a person. For example: 
![here](https://camo.githubusercontent.com/8c4e9cadfa79e1bb5efbe0004f09ee9eba1dffcd2bbf9d500feed05649a7b050/68747470733a2f2f692e696d6775722e636f6d2f6c63656a5977442e676966)

Here is [TDS](https://towardsdatascience.com/remove-body-tattoo-using-deep-learning-4f12942a0fe6) article from the publisher.

Example of AR tool for tattoo placement from [InkHunter](https://apps.apple.com/us/app/inkhunter-try-tattoo-designs/id991558368).



 
##Style Transfer##
###Overview###
Although this was discussed above in the context of StyleGAN, the topic and techiques are much more broad. Research has shown example where a simple "doodle" is transformed into a realistic work of art. Moreover, the artistic style can be specified by the user. One obvious usecase is to customize tattoo designs using this approach. A user could insert a simple design, specify the desired style and the model would about a new custom tattoo.  

Example below is from ()[https://www.tensorflow.org/tutorials/generative/style_transfer]

![Original](https://storage.googleapis.com/download.tensorflow.org/example_images/YellowLabradorLooking_new.jpg)
Original

![Style](https://storage.googleapis.com/download.tensorflow.org/example_images/Vassily_Kandinsky%2C_1913_-_Composition_7.jpg)
Style to match

![Output](https://tensorflow.org/tutorials/generative/images/stylized-image.png)  
Output


