---
layout: post
title: "Applications of Linear Algebra"
modified:
categories: blog
share: true
excerpt: "To help me understand Singular Value Decomposition better, I wrote a little Python script to compress an image using this technique."
tags: [python, algebra]
image:
  feature: feature_image12.jpg
---

For our maths assignment we've been asked to research an application of linear algebra in the field of computing.

The maths this semester has been tough, as I've said before. We've done a ton of linear algebra, particularly matrices, and over the weeks I've been waiting for it to all start to come together, to help me understand what all this stuff has got to do with data science and big data in particular. 

So when the assignment came out I was pleased to have the chance to try and understand a bit more about the context, about why we're learning linear algebra and about how it all fits into a larger picture. When I started doing a bit of research I quickly realised the sheer number of applications linear algebra has. I had no idea.

The particular aspect of linear algebra I decided to focus on was Singular Value Decomposition (SVD), a technique for reducing the rank of a matrix through factorisation. SVD can be applied in a number of areas including:

* Computer vision,
* data mining (e.g. to remove redundant data that isn't required for analysis, from a dataset),
* data compression,
* machine learning (e.g. in face recognition), and 
* data science (e.g. recommender systems).

The application I decided to focus on was using SVD to compress a digital image. To help me understand it better I wrote a short Python program.

The program generates a series of 30 compressed images using the svd function in [numpy](http://www.numpy.org/), stamps each with the singular value from the SVD that's been used to generate it and finally combines all of these images are combined into an animated GIF, so that the viewer can see the efect that the choice of singular values have on the quality of the compressed image.

Here's the resultant animated GIF it produces.

![]({{ "/images/final_landscape.gif" | absolute_url }})

Here's the main part of the code.

{% highlight python %}
image = 'landscape.jpg'
filenames = []

for i in range(1, 30):
	image_name = compressImageUsingSVD(image, i)	
	filenames.append(image_name)

makeVideoFromImages(filenames, 5)
{% endhighlight %}
Let's break it down into steps. The program takes the name of an image and converts the image into a numpy array. It then decomposes it into three matrices. 

{% highlight python %}
def getSVDMatricesForImage(image_name):

	img = Image.open(image_name)
	img = img.convert('LA')

	image_matrix = np.array(list(img.getdata(band=0)), float).reshape(img.size[1], img.size[0])

	image_matrix = np.matrix(image_matrix)

	U,S,V = np.linalg.svd(image_matrix)

	return U,S,V
{% endhighlight %}

It then takes these matrices, and uses the product of parts of them to compress the image. The image is stamped with the singular value used to generate it and written to the disk.

{% highlight python %}
def compressImageUsingSVD(image_name, index):

	U, S, V = getSVDMatricesForImage(image)

	new_matrix = np.matrix(U[:, :index]) * np.diag(S[:index]) * np.matrix(V[:index, :])

	# extract the part of the name before the extension
	fname, fext = splitext(image_name)
	new_img = Image.fromarray(new_matrix)

	# Add the singular value used to compress this image, to the image
	txt = str(index)
	draw = ImageDraw.Draw(new_img)
	draw.text((300, 50), txt)

	image_name = fname + str(index) + '.gif'
	new_img.save(image_name)
	return image_name
{% endhighlight %}

Finally, it takes the series of images and combines them into a single animated GIF.

{% highlight python %}
def makeVideoFromImages(file_names, frame_rate):
	with imageio.get_writer('final_landscape.gif', mode = 'I') as writer:
		for filename in file_names:
			image = imageio.imread(filename)
			# write each 'frame' a number of times to make the image 
			# easier to see
			for i in range(frame_rate):
				writer.append_data(image)
{% endhighlight %}






