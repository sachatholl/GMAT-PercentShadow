# PercentShadow Algorithm for NASA GMAT2025a Orbit Simulator

This repository provides a custom **GMAT UserFunction (GMF)** implementation of the **PercentShadow algorithm**, based on the standard method described by Montenbruck & Gill (2000). The function computes the precise percentage of sunlight received by a spacecraft, taking into account partial and total eclipses caused by an occulting body (e.g. Earth or Moon). The function has been developped and tested on GMAT2025a. The function might also work on previous GMAT versions, but has not been tested!

## What the Code Does

The `IsSunLitGMF` function determines:
- **IsSunLit**: a normalized value (0 = full shadow, 1 = full sunlight).
- **PercentShadow**: the percentage of sunlight received by the spacecraft (0% = full shadow, 100% = full sunlight).

It achieves this by calculating the apparent angular radii of the Sun and the occulting body as seen from the spacecraft, and the angular distance between their centers. The function applies decision logic to determine the illumination state (full sun, umbra, or penumbra).

## How the Algorithm Works

The algorithm:
1. Computes the spacecraft’s position relative to the Sun and the occulting body.
2. Calculates the apparent angular radii of both bodies.
3. Computes the angular distance between their centers.
4. Determines the illumination state:
   - If the disks do not overlap → 100% sunlight.
   - If the occulting body fully covers the Sun → 0% sunlight (umbra).
   - If partial overlap → computes overlap area → derives PercentShadow.

## How to Use

1. Include the `IsSunLitGMF.gmf` file in your GMAT project.
2. In your GMAT script, pass the current positions of the spacecraft, Sun, and occulting body, plus the occulting body’s radius (in km).
3. The function will return `IsSunLit` and `PercentShadow`.
4. Example call in GMAT:
   ```
   [IsSunLitResult, PercentShadow] = IsSunLitGMF(
       MySat.EarthMJ2000Eq.X, MySat.EarthMJ2000Eq.Y, MySat.EarthMJ2000Eq.Z,
       Sun.EarthMJ2000Eq.X, Sun.EarthMJ2000Eq.Y, Sun.EarthMJ2000Eq.Z,
       Earth.EarthMJ2000Eq.X, Earth.EarthMJ2000Eq.Y, Earth.EarthMJ2000Eq.Z,
       6378.137);
   ```
5. Use `PercentShadow` in plots or reports for eclipse, thermal, power analysis or analysis for sensor calibration that depends on ilumination.

**Please include both `IsSunLitGMF.gmf` and `PercentShadowExample.script` in the `script` folder of your GMAT2025a installation. This ensures that GMAT can locate and execute the custom user function together with the example mission script without path issues.**

## Example Simulation: PercentShadowExample.script 

This GMAT simulation demonstrates the use of the custom `IsSunLitGMF.gmf` function within a mission sequence to dynamically assess spacecraft illumination conditions during propagation. As the satellite propagates in its orbit, the script calculates elevation angles, azimuths, and the percent sunlight received (`PercentShadow`) at each step. The function determines whether the spacecraft is fully illuminated, partially shadowed (penumbra), or fully eclipsed (umbra) by analyzing the apparent positions and sizes of the Sun and occulting body.


## Planed improvements:

1. Compute sin rays inscidence angle
2. Scaled incident solar flux effective power


## Presentation

I included a powerpoint presentation (GMAT -PercentShadow_Algorithm_Presentation.pptx) that explains a littlebit more detailed the mechanism of the percent shadow algorithm.

   
Post tenevras spero lucem,
Sacha Tholl



## License

This project is provided under the MIT License.
