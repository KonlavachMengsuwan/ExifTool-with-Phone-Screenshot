# ExifTool-with-Phone-Screenshot

- Delete Camera Parameter
- Delete SunSensor Parameter
- Write Config

Before start:
1. Download and Install Strawberry-perl (https://strawberryperl.com/)
2. Download ExifTool (https://exiftool.org/)
3. Optional download ExifTool GUI (https://exiftool.org/gui/)
4. Download Config (https://github.com/exiftool/exiftool)

I asked from the ExifTool forum: https://exiftool.org/forum/index.php?topic=12742.0;topicseen \
I follow some steps from this youtube video: https://www.youtube.com/watch?v=vTYOC5XamlQ&ab_channel=MidsouthDroneServices

---

## Overall Steps:
Step 1: Backup file /
Step 2: Run ExifTool 
Step 3: Print EXIF and XMP 
Step 4: Export EXIF and XMP as csv 

## Additional:
- Delete Exif 
- List XMP Tag that we want to delete 
- Open pix4d.config and add the XMP Tag that we want to delete 
- Run the code to delete specific XMP 
Done

---

## Step 1: Backup file

---

---

## Step 2: Run ExifTool

1. Unzip "exiftool-12.65.zip"
2. Open "exiftool-12.65" folder
3. Rename "exiftool(-k).exe" to "exiftool.exe"
4. Copy "exiftool.exe" to the image folder
5. Type cmd on the address bar of the image file

---

---

## Step 3: Print EXIF and XMP

```
# Print all Information
exiftool IMG_7019.PNG
exiftool *.PNG

# Print EXIF
exiftool -EXIF:all IMG_7019.PNG
exiftool -EXIF:all *.PNG

# Print XMP
exiftool -XMP:all IMG_7019.PNG
exiftool -XMP:all *.PNG
```
---

---

## Step 4: Export all Metadata into csv file

Write csv:
```
# All information
## One image
exiftool-csv IMG_7019.PNG > IMG_7019.csv

## Every .PNG image in the folder
exiftool -csv *.PNG > Team3_JuanANDOlga_senftenberg.csv

# EXIF Information Only
## One image
exiftool-csv -EXIF:all IMG_7019.PNG > IMG_7019_EXIF.csv

## Every .PNG image in the folder
 exiftool -csv -EXIF:all *.PNG > Team3_JuanANDOlga_senftenberg_EXIF.csv

# XMP Information Only
## One image
exiftool-csv -XMP:all IMG_7019.PNG > IMG_7019_EXIF.csv

## Every .PNG image in the folder
 exiftool -csv -XMP:all *.PNG > Team3_JuanANDOlga_senftenberg_XMP.csv

```

---


# Extra
## Delete Exif
EXIF name to delete
```
"ExposureTime" "FNumber" "ISOSpeed" "ShutterSpeedValue" "ApertureValue"
"ExposureCompensation" "MaxApertureValue"
```

Print before delete
```
# One image TIF
exiftool -EXIF:ExposureTime -FNumber -ISOSpeed -ShutterSpeedValue -ApertureValue -ExposureCompensation -MaxApertureValue DJI_0011.TIF

# One image JPG
exiftool -EXIF:ExposureTime -FNumber -ISOSpeed -ShutterSpeedValue -ApertureValue -ExposureCompensation -MaxApertureValue DJI_0010.JPG

# All images
exiftool -EXIF:ExposureTime -FNumber -ISOSpeed -ShutterSpeedValue -ApertureValue -ExposureCompensation -MaxApertureValue *.TIF
```

Delete all images TIF
```
exiftool -EXIF:ExposureTime= -FNumber= -ISOSpeed= -ShutterSpeedValue= -ApertureValue= -ExposureCompensation= -MaxApertureValue= *.TIF
```

Print after delete
```
# One image
exiftool -EXIF:ExposureTime -FNumber -ISOSpeed -ShutterSpeedValue -ApertureValue -ExposureCompensation -MaxApertureValue DJI_0011.TIF

# all images
exiftool -EXIF:ExposureTime -FNumber -ISOSpeed -ShutterSpeedValue -ApertureValue -ExposureCompensation -MaxApertureValue *.TIF
```

---

## List XMP Tag that we want to delete
Radiometric Calibration parameters
```
"BlackCurrent" "SunSensor" "SunSensorExposureTime" "VignettingPolynomial"
"VignettingCenter" "IsNormalized"
```

Additional
```
"Irradiance" "IrradianceExposureTime" "IrradianceGain" "ExposureTime" 
```

---

## Open pix4d.config and add the XMP Tag that we want to delete
- Use pix4d.config
- Modify 

Make 2 separate config file:
1. Camera
2. DJI

### 1. Camera
Add XMP:Camera parameter that we want to remove
```
%Image::ExifTool::UserDefined::Camera = (
  GROUPS => { 0 => 'XMP', 1 => 'XMP-Camera', 2 => 'Camera' },
  NAMESPACE => { 'Camera' => 'http://pix4d.com/camera/1.0/'  },
  WRITABLE => 'string',
  Yaw             => { Writable => 'real' },
  Pitch           => { Writable => 'real' },
  .
  .
  .
  BlackCurrent => { Writable => 'string' },
  SunSensor => { Writable => 'string' },
  SunSensorExposureTime => { Writable => 'string' },
  
  IsNormalized => { Writable => 'string' },
  # Irradiance => { Writable => 'string' }, ## Because it's drone-dji
  IrradianceExposureTime => { Writable => 'string' },
  IrradianceGain => { Writable => 'real' },
  # ExposureTime => { Writable => 'string' }, ## Because it's drone-dji
);

1;  #end
```

### 2. DJI
Define new drone-dji namespace
```
%Image::ExifTool::UserDefined = (
  'Image::ExifTool::DJI::XMP' => {
    Irradiance => { Writable => 'string' },
    ExposureTime => { Writable => 'string' },
  },
);
```

---

## Run the code to delete specific XMP for Radiometric Calibration in Pix4D
Print before delete
```
# One image TIF
exiftool -XMP:BlackCurrent  -SunSensor -SunSensorExposureTime -VignettingPolynomial -VignettingCenter -IsNormalized -Irradiance -IrradianceExposureTime -IrradianceGain -ExposureTime DJI_0011.TIF

# One image JPG
exiftool -XMP:BlackCurrent  -SunSensor -SunSensorExposureTime -VignettingPolynomial -VignettingCenter -IsNormalized -Irradiance -IrradianceExposureTime -IrradianceGain -ExposureTime DJI_0010.JPG

# All images
exiftool -XMP:BlackCurrent  -SunSensor -SunSensorExposureTime -VignettingPolynomial -VignettingCenter -IsNormalized -Irradiance -IrradianceExposureTime -IrradianceGain -ExposureTime *.TIF
```

DELETE Unwanted Parameter! (Radiometric Calibration)
```
exiftool -config "C:\Users\konla\Downloads\exiftool-master\exiftool-master\config_files\pix4d.config" -XMP:BlackCurrent= -XMP:SunSensor= -XMP:SunSensorExposureTime= -XMP:VignettingPolynomial= -XMP:VignettingCenter= -XMP:IsNormalized= -XMP:IrradianceExposureTime= -XMP:IrradianceGain= -XMP:Irradiance= *.TIF
```

Print after delete
```
# One image
exiftool -XMP:BlackCurrent  -SunSensor -SunSensorExposureTime -VignettingPolynomial -VignettingCenter -IsNormalized -Irradiance -IrradianceExposureTime -IrradianceGain -ExposureTime DJI_0011.TIF

# All images
exiftool -XMP:BlackCurrent  -SunSensor -SunSensorExposureTime -VignettingPolynomial -VignettingCenter -IsNormalized -Irradiance -IrradianceExposureTime -IrradianceGain -ExposureTime *.TIF
```

---

## Remove additional drone-dji (Irradiance, ExposureTime)
Print Irradiance and ExposureTime
```
exiftool -G5:1 -Irradiance -ExposureTime DJI_0011.TIF
exiftool -G5:1 -Irradiance -ExposureTime *.TIF
```

DELETE Irradiance and ExposureTime with new config file
```
exiftool -config "C:\Users\konla\Downloads\exiftool-master\exiftool-master\config_files\pix4d-dji.config" = -XMP:Irradiance= -XMP:ExposureTime= *.TIF
```

Print Irradiance and ExposureTime
```
exiftool -G5:1 -Irradiance -ExposureTime DJI_0011.TIF
exiftool -G5:1 -Irradiance -ExposureTime *.TIF
```

---

## Print Exif and XMP to check
Print Exif
```
exiftool -EXIF:all DJI_0011.TIF
exiftool -EXIF:all *.TIF
```

Print XMP
```
exiftool -XMP:all DJI_0011.TIF
exiftool -XMP:all *.TIF
```

Print Specific XMP
```
exiftool -XMP:BlackCurrent  -SunSensor -SunSensorExposureTime -VignettingPolynomial -VignettingCenter -IsNormalized -Irradiance -IrradianceExposureTime -IrradianceGain -ExposureTime DJI_0011.TIF
exiftool -XMP:BlackCurrent  -SunSensor -SunSensorExposureTime -VignettingPolynomial -VignettingCenter -IsNormalized -Irradiance -IrradianceExposureTime -IrradianceGain -ExposureTime *.TIF
```

---

## Export csv After delete
Write csv
```
exiftool -csv *.TIF > 149_2021_04_29_MS_EXIF_XMP_After.csv
exiftool -csv -EXIF:all *.TIF > 149_2021_04_29_MS_EXIF_After.csv
exiftool -csv -XMP:all *.TIF > 149_2021_04_29_MS_XMP_After.csv
```
