# ApacheSIS_GIGS_Validation

Current Status as of 2022/08/31

0000 - Coordinate reference Systems - 100% 

1000 - General User Documentation - 100%

2100 - Predefined Geodetic Parameter Library - 100%

2200 - Predefined Geodetic Data Objects - 100%
- some test case failures (mostly due to different aliases and some 2208 test case failures) 
- Need some clarification about how to fill out N/A sections on gigs.iogp.org website

3100 - User-Defined Geodetic Parameter Library - 100%

3200 - User-Defined Geodetic Data Objects - 100%
 - some test case failures (difference in alias values and names, custom transformations dont properly set parameters)

5100 - Data Operations (Conversions) 
- in progress, need clarification on gigs criteria
- some test failures (see 5100 Test Failures)

5200 - Data Operations (Coordinate Transformations) - All but one of the test files fails

6000 - Audit Trail - 100%

7000 - Deprecation - 100%



2200 Test Failures

  - 2207 Alias not found for EPSG 2043, 2041, 29871
  - 
  - 2207 name mismatch for EPSG 22091, 22092, 22032, 22033, 27700
  - 
  - 2207 crs not supported for EPSG 22700, 27200, 29701

3200 Test Notes

  - The GeoAPI 3.0 API does not support creation of "User Early-Bound" Datums. This is preventing the validation of the following tests:
  
    - 3205: GIGS_64023 ,GIGS_64277, GIGS_64025, GIGS_64027, GIGS_64313, GIGS_64029, GIGS_64030, GIGS_64031, GIGS_64014, GIGS_64032

    - 3207: GIGS_62031, GIGS_62037, GIGS_62029
    
    - 3208: GIGS_61759, GIGS_61123

    - 3212: GIGS_68178
    
5100 Test Failures (Ignoring conversion issues with USGS Test cases)

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
    
    Converted point : [-2.0000000017631927, 182.1922805493284, 0.0

  - 5108 Cass test case failure:
  
    GIGS-5108-12
    
    Start point: [5.0, 109.0, 0.0]
    
    Expected point: [603116.703, 329668.599, 0.0]
    
    Converted point : [603116.6735686994, 329668.59945207497, 0.0]
    
    Round trip point : [4.999999659340548, 108.99999999755973, 0.0]
    
    Comment: Difference is 3.4E-7° (for a tolerance of 6E-8°). The projection is Cassini-Soldner, which use series expansions. Maybe we should check what is the expected precision for the number of terms provided in the expansion.
    
      


    
