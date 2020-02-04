# NASA-IMPACT datashare program



## Phenomena

### High Latitude Dust
Dust  aerosols  in  the  atmosphere  are  known  to  modulate  the  environmental conditions and climate system through direct and indirect effects between theland–atmosphere–ocean system. HLD as ”particles that are deflated from a surface and travel by suspension in the atmosphere”, having sizes that spawn from tenths of μm to larger aggregates.In addition,  dust events are considered high latitude when they are ≥ 50◦N and ≥40◦S. HLD  tends  to  occur  under  specificconditions  having  a  more  seasonal  dependence  and  can  persist  from  hours  toa few days.

![High Latitude Dust Sample](/examples/hld_sample.jpg)

Data Description:
1. Each event in a folder.
2. Folder Structure:
  ```
   high-latitude-dust_<id>:
     |> high-latitude-dust_<id>.dbf
     |> high-latitude-dust_<id>.prj
     |> high-latitude-dust_<id>.shp
     |> high-latitude-dust_<id>.shx
  ```
3. Images to be downloaded using GIBS. (https://gibs.earthdata.nasa.gov/wms/epsg4326/best/wms.cgi?SERVICE=WMS&REQUEST=GetMap&layers=MODIS_Aqua_CorrectedReflectance_TrueColor&version=1.3.0&crs=EPSG:4326&transparent=false&width={}&height={}&bbox={}&format=image/tiff&time={})
3.a. Variables required:
     > width = width of the image
     > height = height of the image
     > bbox = [left_latitude, left_longitude, right_latitude, right_longitude]

*Note: Use the following for width, height calculation and url generation.*

```
URL = "https://gibs.earthdata.nasa.gov/wms/epsg4326/best/wms.cgi?SERVICE=WMS&REQUEST=GetMap&layers=MODIS_Aqua_CorrectedReflectance_TrueColor&version=1.3.0&crs=EPSG:4326&transparent=false&width={}&height={}&bbox={}&format=image/tiff&time={}"
KM_PER_DEG_AT_EQ = 111.


def calculate_width_height(extent, resolution):
    lats = extent[::2]
    lons = extent[1::2]
    km_per_deg_at_lat = KM_PER_DEG_AT_EQ * np.cos(np.pi * np.mean(lats) / 180.)
    width = int((lons[1] - lons[0]) * km_per_deg_at_lat / resolution)
    height = int((lats[1] - lats[0]) * KM_PER_DEG_AT_EQ / resolution)
    print(width, height)
    return (width, height)


def modis_url(time, extent, resolution):
    width, height = calculate_width_height(extent, resolution)
    extent = ','.join(map(lambda x: str(x), extent))
    return (width, height, URL.format(width, height, extent, time))
```

You can download the files from: `s3://hls-datashare`. You will need an AWS account to download the data.

### Cloud Street

#### Background

The organization of cumulus clouds into elongated rows oriented parallel to the mean boundary layer flow is a phenomenon often referred to as cloud streets. Organized cumulus cloud streets are the visual manifestation of underlying boundary layer roll circulations commonly referred to as horizontal convective rolls. Formation of boundary layer rolls is commonly attributed to  two instability mechanisms – thermal and dynamic instability.

Direct impacts of cloud streets are fairly minimal as they typically do not precipitate or have meaningful environmental impacts aside from surface radiation.  The effects of cloud streets are most notable in the presence of sea breeze circulations as intersections of roll circulations and sea breeze circulations are known to force convective initiation.  Coastal convection is a primary source of precipitation in these settings and can grow upscale to propagating mesoscale convective systems that modulate the regional and even global precipitation budgets.

![Cloud Street Sample](/examples/cloudstreet_sample.jpg)


#### Data Description:

1. Folder Structure:
  ```
   Cloud Streets/
       |> Yes
          |> <images>
       |> No
          |> <images>
  ```
2. Data type: images, jpg

You can download the files from: `s3://cloudstreet-datashare`. You will need an AWS account to download the data.