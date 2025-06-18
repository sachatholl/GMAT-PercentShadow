# PercentShadow Algorithm for GMAT

Coded by Sacha Tholl for GMAT 2025

This repository provides a custom **GMAT UserFunction (GMF)** implementation of the **PercentShadow algorithm**, based on the standard method described by Montenbruck & Gill (2000). The function computes the precise percentage of sunlight received by a spacecraft, taking into account partial and total eclipses caused by an occulting body (e.g. Earth or Moon).

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
       OTTER.EarthMJ2000Eq.X, OTTER.EarthMJ2000Eq.Y, OTTER.EarthMJ2000Eq.Z,
       Sun.EarthMJ2000Eq.X, Sun.EarthMJ2000Eq.Y, Sun.EarthMJ2000Eq.Z,
       Earth.EarthMJ2000Eq.X, Earth.EarthMJ2000Eq.Y, Earth.EarthMJ2000Eq.Z,
       6378.137);
   ```
5. Use `PercentShadow` in plots or reports for eclipse and power analysis.

## License

This project is provided under the MIT License.
