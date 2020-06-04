# Open Source Architecture Workflows

Notes on open source workflows within the architecture/building/construction industry

## Surveying

### Deciding on a survey type

When deciding on a survey type, the main factors are probably legal requirements, accuracy/coverage and cost.

Many jurisdictions have legal requirements that licensed surveyors output are submitted in order to approve construction work, which eliminates the option of self-survey. However, __traditional surveyors__ are often expensive, slow (2 weeks to get data is considered fast in some markets!) and low resolution, and may have other problems such as spotty coverage and inaccurate approximations interspersed with precise positioning information. While their primary point readings are accurate, their coverage of the space will be really basic and spotty.

__DIY surveying__ with video output from a phone or a drone is possible with open source photogrammetry tools, and will result in point cloud data which can be further processed. Photogrammetry based point cloud surveys are also the norm for large commercial and insturial sites which require large scale surveying. Commercial __point cloud surveyors__ increasingly tend to use cloud based services (drone operator uploads video to third party, waits, downloads dataset). When executed propertly, point clouds provide far better coverage of a site at good accuracy.

All surveys may be complicated by certain types of features (eg. dense foliage).

Finally, you may not need a survey at all. Satellite images can be obtained online for free, and existing site plans can often be obtained from local governments, which increasingly have GIS systems or web GIS portals for this purpose.

![image](surveying.svg)
