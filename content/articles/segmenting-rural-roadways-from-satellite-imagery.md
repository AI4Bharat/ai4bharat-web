---
title: "Challenge: Segmenting Rural Roadways from Satellite Imagery"
date: 2019-06-01T10:07:17+06:00
# post image
image: "images/projects/segmenting-rural-roadways-from-satellite-imagery/Thumbnail.png"
# post type (regular/featured)
type: "regular"
# meta description
description: "Interconnectivity in rural and semi-urban areas is required to understand progress of road construction and also for planning expansions."
# post draft
draft: false
---

Interconnectivity in rural and semi-urban areas is required to understand progress of road construction and also for planning expansions. Segmenting khachcha roads from satellite imagery is remains an unsolved challenge.

---

### Problem Statement

Segmentation of Indian Rural Roadways and extraction of road networks using Satellite Imagery for path planning and other applications.

### Why is this relevant in the Indian context

* India is a rapidly developing country, and Roadways play a very significant role in the development. Developments of sectors like Manufacturing industries, agricultural industries involve proper planning based on the availability of transportation facilities. The roadways are inevitable contributors. Thus Proper understanding of road networks becomes mandatory. Roadways in India is increasing exponentially as a necessity, and requires faster ways of updating the existing databases and monitoring for changes with minimal or no human involvement is expected.

<figure style="text-align: center">
  <img src="../../images/projects/segmenting-rural-roadways-from-satellite-imagery/NH_lengths.png" style="width:60%;">
  <figcaption>Source: data.gov.in</figcaption>
</figure>

* Around 70% of Indians live in rural areas (according to 2011 census). It is mandatory to develop proper infrastructure for the entire population, which is carried out using Pradhan Mantri Gram Sadak Yojana. GIS updates of such road regions require a level of manual effort, for India (3.28 Million km2 ) continuous update of changes is difficult.

<figure style="text-align: center">
  <img src="../../images/projects/segmenting-rural-roadways-from-satellite-imagery/road_lengths.png" style="width:60%;">
  <figcaption>Source: data.gov.in</figcaption>
</figure>

* For rural road management identification of existing roads in rural areas for with traditional techniques is difficult since around 40% of rural roads are unsurfaced (like dirt roads) [based on 2015 stats], this makes the problem even worse and requires a lot of man-hours to be invested.

<figure style="text-align: center">
  <img src="../../images/projects/segmenting-rural-roadways-from-satellite-imagery/surfaced_roads.png" style="width:60%;">
  <figcaption>Source: data.gov.in</figcaption>
</figure>

* For Disaster Management and Recovery,  Roadways are critical for the immediate and first-level response. In this scenario, exciting infrastructures would have been damaged. It becomes important to identify existing functional routes post the disaster and identify the alternative routes, to help in accelerated recovery actions.
* India has a large area of ghats and mountainous regions, which are prone to frequent landslides, it is difficult yet crucial to obtain information at the earliest. There are areas near coastal regions and Central India are prone to cyclones and flooding fall under high to very high risk zones.

<table style="border: none;">
  <tr>
    <td>
        <figure style="text-align: center">
            <img src="../../images/projects/segmenting-rural-roadways-from-satellite-imagery/landslides.png">
        </figure>
    </td>
    <td width="44%">
      <figure style="text-align: center">
          <img src="../../images/projects/segmenting-rural-roadways-from-satellite-imagery/wind.png">
      </figure>
    </td>
  </tr>
</table>

Source: [United Nations Development Program](https://www.undp.org/) - [Disaster management in India](https://www.undp.org/content/dam/india/docs/disaster_management_in_india.pdf)

In these scenarios existing GIS databases are of little or no use since changes in terrain and geographical features would have changed post calamity. So only means of immediate insights can be obtained if through satellite imagery.

### Data Availability And Collection

These are list of existing satellite images with corresponding road networks annotated datasets available for public use

1. SpaceNet Challenge [Dataset](https://spacenetchallenge.github.io/datasets/spacenetRoads-summary.html)
  * contains roads annotated for satellite images crops of Shanghai, Khartoum, Paris, and Vegas
  * the data covers around 3011 sq. km and length of 8677 km of annotated roads
2. DeepGlobe Dataset [12]
  * Roadways annotated images captured over Thailand, Indonesia, and India
  * road dataset consists of a total of 8570 images and spans a total land area of 2220 sq. km
3. International Society for Photogrammetry and Remote Sensing ([ISPRS](https://www.isprs.org/publications/archives.aspx) )- semantic labeling benchmark dataset
4. University of Toronto - contains [dataset](https://www.cs.toronto.edu/~vmnih/data/) road for Massachusetts 
5. Open Street Maps
  * Not a dataset, but a crowd-sourced collection of map tags from where one can get the info on road networks

Most of the state-of-the-art papers on this topic seem to use at least one of these datasets, with refinement of ground truth if necessary.

[Landsat](https://www.usgs.gov/land-resources/nli/landsat) of USA and [Sentinel](https://sentinel.esa.int/web/sentinel/home) of ESA provides free satellite imagery for all location on earth.

For Indian maps, we can use our own ISRO satellite’s data, called [Bhuvan GIS Data](https://bhuvan.nrsc.gov.in/bhuvan_links.php). After registering on the website, it’s possible to select a region of interest in the maps, called a tile, and download the corresponding Satellite imagery along with the GIS data for the tile(s).

However, the GIS data does not include roadways, which is crucial as the ground truth of our model. We may have to manually annotate the roadways, or use some existing models for creating an initial outline of the road maps to start with, on which we have to do a lot of refinement.

To overcome this, as an initial step, we can use the road map info that’s already crowd-sourced in the OpenStreetMaps [for India](https://wiki.openstreetmap.org/wiki/India:Roads). The tag format can be found [here ](https://wiki.openstreetmap.org/wiki/India:Tags/Highway)for roadways.

Note: OSM have collected Indian satellite imagery from different sources and combined them all according to the latest ones with high resolution. The list of all sources for Indian Satellite Imagery can be found [here](https://wiki.openstreetmap.org/wiki/India/Data_sources).

In addition, we have another source of getting map data, called [WikiMapia](http://wikimapia.org/). WikiMapia is owned by a private company, but provides data that can be used for free. They use satellite imageries directly from Google Maps, on which they add other layers, in which roadlines is one.

Although the only way of collecting data from WikiMapia is their APIs, the roadways in WikiMapia have better annotations than OSM, and has good rural area coverage. But the APIs seem to be quite slow, and it might take a very large amount of time to get a good amount of data for covering the entire Indian subcontinent.

### Existing Work - Research And Practice

Prior to deep learning the procedure for road extraction [1] roughly follows steps

1. Pre processing
2. Detection of edges corresponding to roads
3. Road following 
4. Grouping of road primitives

Lot of research has been put in crafting state of art algorithms to solve each piece of these steps by means computer vision and statistical methods. Ensembles of algorithms were also used to high accuracy and fault-tolerant methods [2]. Geometric-probabilistic models are used to automatically extract road networks like map (maximum aposteriori probability) estimation [4], Monte Carlo dynamics [5], kalman and particle filters [6]. 

For evaluating and validating the results metrics [3] like completeness, correctness, redundancy, gaps in connectivity.

Few of the research works have explored and incorporated learning algorithms, e.g random forests along with Conditional Random Fields [7]. A simple Artificial Neural network (FC) was used to pixel-level classification [8] for class road or not road, where RGB data of pixels was fed to the network.

Then with the advent of Deep Neural networks, end to end systems entirely with neural nets and pre/post-processing stages is being explored. [DEEPGLOBE](http://deepglobe.org/), challenge hosted by CVPR18 workshop, here the problem is addressed as pixel-wise segmentation problem and uses U-Net[11] based architecture for models. The model with top result uses LinkNet-based architecture with dilated convolutions called D-Linknet [9] . Multiple challenges hosted by [SpaceNet](https://spacenetchallenge.github.io/Challenges/challengesSummary.html) [10] have notable contributions with good accuracy.

### Open Technical Challenges

Satellite Imagery processing for Geographical understanding is done for more than 4 decades. Identifying and segmenting Road networks is one of the important problems addressed. But all the processing methods and algorithms are hand-crafted for a specific purpose under specific contexts with pieces of information added by human operators. This requires manpower and human intervention.

1. Computer Vision based processing methods relies on prior knowledge and understanding of the locality (obtained by other means like GIS). Having such prior knowledge in all the scenarios is not feasible (e.g at times like natural disaster). Some of them also require human intervention for initializing certain parameters, validating and correcting the outputs from time to time
2. Deep Learning models generally learns inherent details and underlying correlations among them, and generalize to the context. This seems to offer a valid solution, but there are few challenges to be addressed like:
  * Good, Clean dataset with proper annotation of road maps and This must be as diverse as possible covering wide range contexts. Context of farmlands, deserts, sub-urban, forest mountain regions, riverbanks and delta regions, all are different.
  * Deep learning Model should be able to generalize between different contexts since many rural areas and towns include small chunks of residences similar to urban regions surrounded or intertwined with farmlands and forestry
  * DL Model should be able to understand the features in the given image. In satellite images, all the objects of interest will be small so it is important for the model to understand contexts and objects from pixel information to properly classify which are roads and which are not.
  * Measuring Accuracy and Loss functions requires metrics different from conventional Classification and Segmentation problems. Traditional metrics used for evaluating road networks extraction tasks in CV methods, should be adapted to suit learning algorithms and incorporated Deep Learning methods.

### Technical Milestones

1. Survey on all existing approaches, including both CV-based methods and deep learning based end-to-end solutions.

2. Survey on all available sources of annotated data. << In Progress >>

To Do:

3. Planning on how to start collecting data, and the legal access to use them for open sourcing.

4. Organizing the satellite data along with the tags for all the required regions.

5. Analyzing and cleaning the dataset, and processing it to add missing roads, etc. using automatic annotation if possible, which should be subject to further refinement.

6. Creating an API-level interaction to access the dataset (that is, the satellite images and corresponding annotations), providing a good abstraction for training, etc.

7. Designing a robust training architecture to train a deep learning based model for the task of road segmentation, and the pre-processing techniques involved to refine the predictions.

8. If there are any challenges/difficulties specific to rural roadways, address them individually if possible to make the model robust to any type of roads.

9. Test the designed network on a systematic basis to evaluate the performance and decide the fitness of the model for real-time deployment to generate the entire road way map of India.

10. Optional: Create an Android app for the resulting Maps generated out of the model, which will be very helpful for rural navigation.

### References

[1] Fortier, M.A., Ziou, D., & Armenakis, C. (2002). Survey of Work on Road Extraction in Aerial and Satellite Images.

[2] McKeown Jr, D.M. & Denlinger, J.L.. (1988). Cooperative methods for road tracking in aerial imagery. 662 - 672. 10.1109/CVPR.1988.196307. 

[3] Heipke, C., Mayer, H.J., Wiedemann, C., & Jamet, O. (1998). Empirical Evaluation of Automatically Extracted Road Axes.

[4] Barzohar, M., & Cooper, D. B. (1996). Automatic finding of main roads in aerial images by using geometric-stochastic models and estimation. IEEE Transactions on Pattern Analysis and Machine Intelligence, 18(7), 707-721.

[5] Stoica, R., Descombes, X., & Zerubia, J. (2004). A Gibbs point process for road extraction from remotely sensed images. International Journal of Computer Vision, 57(2), 121-136.

[6] Movaghati, S., Moghaddamjoo, A., & Tavakoli, A. (2010). Road extraction from satellite images using particle filtering and extended Kalman filtering. IEEE Transactions on geoscience and remote sensing, 48(7), 2807-2817.

[7] Wegner, J. D., Montoya-Zegarra, J. A., & Schindler, K. (2013). A higher-order CRF model for road network extraction. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (pp. 1698-1705).

[8] Mokhtarzade, M., & Zoej, M. V. (2007). Road detection from high-resolution satellite images using artificial neural networks. International journal of applied earth observation and geoinformation, 9(1), 32-40.

[9] Zhou, L., Zhang, C., & Wu, M. (2018, June). D-linknet: Linknet with pretrained encoder and dilated convolution for high resolution satellite imagery road extraction. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition Workshops (pp. 182-186). IEEE.

[10] Van Etten, A. (2019). City-scale Road Extraction from Satellite Imagery. arXiv preprint arXiv:1904.09901.

[11] Ronneberger, O., Fischer, P., & Brox, T. (2015, October). U-net: Convolutional networks for biomedical image segmentation. In International Conference on Medical image computing and computer-assisted intervention (pp. 234-241). Springer, Cham.

[12] Demir, I., Koperski, K., Lindenbaum, D., Pang, G., Huang, J., Basu, S., ... & Raska, R. (2018, June). Deepglobe 2018: A challenge to parse the earth through satellite images. In 2018 IEEE/CVF Conference on Computer Vision and Pattern Recognition Workshops (CVPRW) (pp. 172-17209). IEEE.

### Current Team

<table style="width:50%; margin-right:50%;">
  <tr>
    <td align="center" >
      <img src="../../images/pratyush.png" width="75px;" class="rounded-circle" style="margin-bottom: 0;"/>
      <br/><b>Mentor</b>
    </td>
    <td align="center" style="vertical-align: middle;"> <h4>
      Pratyush Kumar
      <br/>
      IIT-Madras
      <br/>
      (<a href="https://www.linkedin.com/in/pratyush-kumar-8844a8a3/" style="color: orange">LinkedIn</a>)
    </h4></td>
  </tr>
  <tr>
    <td align="center" >
      <img src="https://github.com/JosephGeoBenjamin.png" width="75px;" class="rounded-circle" style="margin-bottom: 0;"/>
      <br/><b>Lead</b>
    </td>
    <td align="center" style="vertical-align: middle;"> <h4>
      Joseph Geo
      <br/>
      ML Engineer, One Fourth Labs
      <br/>
      (<a href="https://www.linkedin.com/in/joseph-geo-benjamin/" style="color: orange">LinkedIn</a>)
    </h4></td>
  </tr>
  <tr>
    <td align="center" >
      <img src="https://github.com/GokulNC.png" width="75px;" class="rounded-circle" style="margin-bottom: 0;"/>
      <br/><b>Lead</b>
    </td>
    <td align="center" style="vertical-align: middle;"> <h4>
      Gokul NC
      <br/>
      ML Engineer, One Fourth Labs
      <br/>
      (<a href="https://www.linkedin.com/in/gokulnc/" style="color: orange">LinkedIn</a>)
    </h4></td>
  </tr>
</table>
