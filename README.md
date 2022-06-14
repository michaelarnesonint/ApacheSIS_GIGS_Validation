# ApacheSIS_GIGS_Validation

Current Status as of 2022/06/14

0000 - Coordinate reference Systems - 100% 

1000 - General User Documentation - 100%

2100 - Predefined Geodetic Parameter Library - 100%

2200 - Predefined Geodetic Data Objects - 100%
- some test case failures (mostly due to different aliases and some 2208 test case failures) 
- Need some clarification about how to fill out N/A sections on gigs.iogp.org website

3100 - User-Defined Geodetic Parameter Library - 100%

3200 - User-Defined Geodetic Data Objects - Tests 3202-3204, 3206, 3207 are done, 3205, 3208 are currently in progress. Tests 3201, 3209-3212 still need to be implemented.

5100 - Data Operations (Conversions) 
- in progress, need clarification on gigs criteria
- some test failures (see 5100 Test Failures)

5200 - Data Operations (Coordinate Transformations) - in progress, majority of test cases fail

6000 - Audit Trail - 100%

7000 - Deprecation - in progress



2200 Test Failures
  - 2207 Alias not found for EPSG 2043, 2041, 29871
  - 2207 name mismatch for EPSG 22091, 22092, 22032, 22033, 27700
  - 2207 crs not supported for EPSG 22700, 27200, 29701


5100 Test Failures 

  - 5111 GIGS_conv_5111_MercA_part1
  
    GIGS-5111-19
    
    Start point: [2.376410584E7, 679490.646, 0.0]
    
    Expected point: [-2.0, -71.0, 0.0]
    
    Converted point : [-2.0000000017631927, 288.9999999937728, 0.0]
    
    Comment: This is a 360° wraparound on the longitude value. The 2 points are equivalent. I do not know if the tests should accept or not longitude differences that are multiple of 360°. My policy in existing GIGS tests has been to accept them, so the choice of subtracting 360° or not is left to implementations. While I understand that users would expect -71° when transforming a single point, in the context where we transform many points (e.g. when transforming a geometry or resampling a raster), subtracting 360° suddenly in the middle of a geometry causes a lot of problems. So this is not something that we want to do in all cases, and we may need some mechanism for giving the choice to users. In the meantime, it is possible to make the tests tolerance to 360° shift by involing the following method on every longitude values:
    
     import org.apache.sis.measure.Longitude;
     
     λ = Longitude.normalize(λ);

  - 5111 MercA_part2 test case failure:
  
    GIGS-5111-54
    
    Start point: [2.376410584E7, 679490.646, 0.0]
    
    Expected point: [-2.0, -177.8077194, 0.0]
    
    Converted point : [-2.0000000017631927, 182.1922805493284, 0.0]

  - 5101 TM_part1_USGS test case failure:
  
    GIGS-5101-02
    
    Start point: [678711.584, 1134498.83, 0.0]
    
    Expected point: [60.0, 3.0000003, 0.0]
    
    Converted point : [60.00000000126028, 2.999999994240299, 0.0]
    
    Comment: In this case the difference (3.06E-7°) is close to the tolerance threshold (3.0E-7°). It is possible that providing more decimal in the start point would solve the issue.
    
  - 5101 TM_part2_USGS test case failure:
  
    GIGS-5101-70
    
    Start point: [110043.299, 6672079.494, 0.0]
    
    Expected point: [60.0, -4.0000025, 0.0]
    
    Converted point : [59.9999999989898, -3.9999999941406825, 0.0]
    
    Comment: The difference is 2.5E-6° degrees. In GIGS 1, I noticed that some tests failures were caused by missing digits in the parameter values compared to the values in EPSG database. I do not know if it still the case in GIGS 2 or in the tests used here. If not the case, we would need to investigate more.
    
  - 5101 TM_part3_USGS test case failure:
  
    GIGS-5101-103
    
    Start point: [834359.668, 3333406.428, 0.0]
    
    Expected point: [-60.0, 147.0000008, 0.0]
    
    Converted point : [-59.99999999969919, 147.00000000180773, 0.0]
    
    Comment: Difference is 8E-7° (for a tolerance of 3E-7°). Same comment than above.
    
  - 5101 TM_part4_USGS test case failure:
  
    GIGS-5101-115
    
    Start point: [-40.000215, -70.0002089, 0.0]
    
    Expected point: [5524200.115, 4645299.925, 0.0]
    
    Converted point : [5524200.122733561, 4645300.111994601, 0.0]
    
    Comment: Difference is 0.19 meter (for a tolerance of 0.03 meter). Same comment than above, applied to the projected space. Note: all those TM tests use the Transverse Mercator projection, which has different formulas. The formula in EPSG guidance notes are more recent (and more accurate) than the formula published in Snyder's book. We may need to check which formula was used for the expected points.

  - 5108 Cass test case failure:
  
    GIGS-5108-12
    
    Expected point: [603116.703, 329668.599, 0.0]
    
    Converted point : [603116.6735686994, 329668.59945207497, 0.0]
    
    Round trip point : [4.999999659340548, 108.99999999755973, 0.0]
    
    Comment: Difference is 3.4E-7° (for a tolerance of 6E-8°). The projection is Cassini-Soldner, which use series expansions. Maybe we should check what is the expected precision for the number of terms provided in the expansion.
    
      


    
