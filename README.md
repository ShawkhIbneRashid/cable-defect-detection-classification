<html>

<body>
  <h3>Problem Statement</h3>
<p align="justify">We will measure the diameters of the cables in pixels from two different points. Then we will circle the defects (Cut, scratch or pin hole) on each cable image and finally we will classify all the defects in the images. </p>
<b>Programming Environment:</b> C++ with OpenCV version 4.5.5. Used Visual Student 2022 IDE.
<h3>To Run the Code</h3>
<ol>
  <li>Create a new empty C++ project in Visual Studio and create a new C++ file. </li>
  <li>Paste the source code (main.cpp) into the created file. </li>
  <li>Change some properties to run the code. Go to Project -> Properties -> select Release in Configuration field -> expand C/C++ -> select General -> add include folder of OpenCV in the Additional Include Directories field. </li>
  <li>Then go to Linker -> General -> Additional Library Directories and include the directory of lib folder which is inside vc15 folder. </li>
  <li>After that go to Linker -> Input and add the lib file (add the first one) of OpenCV which is inside lib folder of VC15 in Additional Dependencies field. </li>
  <li>Click ok and change the Solution Configuration from Debug to Release. Then you can build and run the program. The bin folder of OpenCV needs to be in the system environment variables for the code to run.</li>
</ol>

<h3>My Approach</h3>
<h4>a.	Measuring the diameter of the cable</h4>
  <p align="justify">
The original image is of type BMP. At first, I converted the BMP image to grayscale image. 
  </p>

  <a href="https://drive.google.com/uc?export=view&id=1XdXrh-Y0jw0nnxDaQFK56z83urrV7x0w"><img src="https://drive.google.com/uc?export=view&id=1XdXrh-Y0jw0nnxDaQFK56z83urrV7x0w" style="width: 600px; height: 400px; float: left" title="Converted the BMP image to grayscale image." />
  <p>Figure 1: Converted the BMP image to grayscale image.</p>
  <p align="justify">Then I made the pixels outside of the center 240 pixels black to remove noise. I did not change the pixels at the center because the cable is at the center of the image. I used Canny edge detection algorithm to detect the edges from the grayscale image. </p>
  
  
  <a href="https://drive.google.com/uc?export=view&id=1HSHQn2BuxGBMFGPcPZoHDEfHu7djM5Lx"><img src="https://drive.google.com/uc?export=view&id=1HSHQn2BuxGBMFGPcPZoHDEfHu7djM5Lx" style="width: 600px; height: 400px; float: left" title="Noises" />
  <p>Figure 2: Noise present in the grayscale image and the detected edges image. After setting the pixels to white color in the grayscale image outside of the center 240 pixels, the noises were removed.</p>
  <p align="justify">Then, I measured the diameter of the cable at two different points. To do that I first got the first pixel and the last pixel of the cable at a particular row. Then from those two points, I got the diameter of the cable at a particular point/row. In the first figure, the diameters of the two points are 83 and 79 pixels and on the second cable the diameters are 68 and 67 pixels.</p>
  
  <a href="https://drive.google.com/uc?export=view&id=1MdMESDueLRekeJZee7UbAjl6UA1_Nxdm"><img src="https://drive.google.com/uc?export=view&id=1MdMESDueLRekeJZee7UbAjl6UA1_Nxdm" style="width: 600px; height: 400px; float: left" title="Diameters" />
  <p>Figure 3: Images showing diameter of the cable at three different points for three of the four cable images.</p>
  
  <h4>b.  Circling the defects of each cable image</h4>
  <p align="justify">I detected the defects by first finding out the contours from the edge images. Then I approximated polygons from the contours and found minimum enclosing circles for every polygon and saved the radiuses and centers for those circles. I then drew circles around the polygons. But the problem was there were other polygons in the edge image, such as cable, unnecessary polygons detected close to the defects, and polygons around the noises which are outside of the cable. </p>

  <a href="https://drive.google.com/uc?export=view&id=1LhwmZ8BqLFlTEi9sICVrrZfzMB1zbHX9"><img src="https://drive.google.com/uc?export=view&id=1LhwmZ8BqLFlTEi9sICVrrZfzMB1zbHX9" style="width: 600px; height: 400px; float: left" title="Diameters" />
  <p>Figure 4: Circles drawn around the polygons. As we can see, there are polygons other than the defects in the images.</p>
  <p align="justify">To remove the circles around unnecessary polygons, I first removed the circle around the cable. I did that by first checking for the maximum radius and made it to zero so that no circle can be drawn. I further removed some of the circles (which are mainly noises), whose radius fall under a certain threshold. Later I got the new largest circle (previously the circle around the cable was the largest one) and removed the circles which intersect, touch, or within the largest circle. I did that by calculating the distance between the centers of the largest circle and another circle that is being considered. This helped to remove unnecessary circles. </p>
 
  <a href="https://drive.google.com/uc?export=view&id=17vi3APtF5bKteBwrW27thhjMNgN34kJZ"><img src="https://drive.google.com/uc?export=view&id=17vi3APtF5bKteBwrW27thhjMNgN34kJZ" style="width: 600px; height: 400px; float: left" title="Diameters" />
  <p>Figure 5: After removing unnecessary circles from the images, we can see the circles around the defects only.</p>
  
  <h4>c.	Classifying each defect as either Pin Hole, Cut, or Scratch</h4>
  <p align="justify">I classified the defects based on the radiuses of the circles. If the radius is within a certain threshold then I classified the defects as a certain type. For example, if the radius was less than or equal to 10, I classified the defect as Pin Hole type. If the radius was greater than 10 and less than or equal to 40, I marked the defect as Cut, and for a radius greater than 40, I classified it as Scratch.</p>

  <a href="https://drive.google.com/uc?export=view&id=1J_LxReKc2mzLgGOb094XLPcDmZ0XCI13"><img src="https://drive.google.com/uc?export=view&id=1J_LxReKc2mzLgGOb094XLPcDmZ0XCI13" style="width: 600px; height: 400px; float: left" title="Diameters" />
  <p>Figure 6: Classified defects from two cable images.</p>
</body>
</html>
