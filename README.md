# LUR Artillery

**LUR Artillery** is a community-supported firing calculator for the game WARDOGS. It combines map coordinates, weapon range tables, field calibration and confirmed player observations to help estimate range and firing elevation.

Developer: **Lur (Lurcat)** · [Discord community](https://discord.gg/wardogskoth)

## How a firing solution is formed

The map uses a tactical coordinate grid where one coordinate unit represents approximately 100 metres. For gun position \((X_g,Y_g)\) and target position \((X_t,Y_t)\):

- Horizontal range: \(D=100\sqrt{(X_t-X_g)^2+(Y_t-Y_g)^2}\) metres.
- Azimuth, measured clockwise from north: \(A=(\operatorname{atan2}(X_t-X_g,Y_t-Y_g)\cdot180/\pi+360)\bmod360\) degrees.

The selected weapon and trajectory provide a range-to-elevation table. Between adjacent table entries \((r_0,m_0)\) and \((r_1,m_1)\), elevation in mils is linearly interpolated:

\[
M(r)=m_0+\frac{r-r_0}{r_1-r_0}(m_1-m_0)
\]

The calculator checks the weapon's usable range and displays a firing solution or an out-of-range result. Supported SPH-2 low-trajectory map profiles can add a bounded empirical field correction. Between two confirmed calibration points, the correction follows the same linear interpolation rule and fades toward zero outside the calibrated range. This is a measured game-specific adjustment, not a universal ballistic law.

## Internal elevation surface

The 2D map is paired with an internal, normally invisible elevation surface derived from its height data. Map coordinates are transformed to heightmap coordinates; the height between four neighboring samples is interpolated bilinearly. If \(q\) is the normalized 16-bit height value:

\[
z=z_{min}+\frac{q}{65535}(z_{max}-z_{min})
\]

For neighboring heights \(h_{00},h_{10},h_{01},h_{11}\) and fractional position \((u,v)\):

\[
h(u,v)=(1-u)(1-v)h_{00}+u(1-v)h_{10}+(1-u)vh_{01}+uvh_{11}
\]

This surface supplies elevation at the gun and target positions and supports terrain diagnostics. The current firing elevation remains based on the selected range table and applicable field calibration; the height surface is not itself a learned shot-correction grid.

## What player observations contribute

After a player records a confirmed adjustment series, its map, weapon, trajectory, positions and measured correction can improve the local reference model. One continuous series counts as one independent confirmation; repeated shots in that series refine its result instead of counting as separate confirmations.

Players may optionally include reviewed Community observations in the calculator. Community submissions contain selected gameplay measurements and a pseudonymous app ID used to keep contributors' records separate. After review, accepted measurements can enrich the shared reference model. They do not include technical diagnostic journals.

Technical logs are a separate, optional diagnostic record. They may contain event times, map and gameplay coordinates, app settings and device details. They help investigate bugs and plan fixes; they are not inputs to firing calculations or Community data.

To request a code for optional Community or technical-log submissions, contact **Lurcat** in the [Discord community](https://discord.gg/wardogskoth).

## Project status and affiliation

This project is an independent community tool and is not affiliated with or endorsed by the WARDOGS publisher. Game updates or changes to maps and weapon behavior may make observations or estimates outdated. Verify results in the game.

The accompanying license applies to LUR Artillery and its original materials. It does not claim exclusive ownership of mathematical ideas, formulas, game facts or independently created methods.

---

# LUR Artillery (English)

LUR Artillery is a community-supported firing calculator for WARDOGS. It combines map coordinates, weapon range tables, field calibration and confirmed player observations to estimate range and firing elevation.

## Calculation summary

One tactical map coordinate unit represents approximately 100 metres. Given gun \((X_g,Y_g)\) and target \((X_t,Y_t)\):

- Range: \(D=100\sqrt{(X_t-X_g)^2+(Y_t-Y_g)^2}\) metres.
- Azimuth clockwise from north: \(A=(\operatorname{atan2}(X_t-X_g,Y_t-Y_g)\cdot180/\pi+360)\bmod360\) degrees.

The selected weapon and trajectory table maps range to elevation in mils. Between neighboring entries, the calculator uses linear interpolation: \(M(r)=m_0+(r-r_0)(m_1-m_0)/(r_1-r_0)\). Supported SPH-2 low-trajectory profiles may add a bounded, map-specific correction based on confirmed field measurements. It fades toward zero beyond its calibrated range.

## Invisible elevation surface and observations

An internal elevation surface is sampled over the 2D map using bilinear interpolation of neighboring heightmap values. A normalized 16-bit value \(q\) maps to elevation by \(z=z_{min}+q(z_{max}-z_{min})/65535\). This gives terrain heights at gun and target positions for diagnostics; the current firing elevation still comes from the selected range table and applicable field calibration.

Confirmed player adjustment series enrich the local empirical reference model. A continuous series is one independent confirmation; additional shots in that series refine its result. Optional, reviewed Community observations can enrich the shared model, while technical logs remain separate diagnostics and never enter firing calculations.

For a submission code, contact **Lurcat** in the [Discord community](https://discord.gg/wardogskoth). LUR Artillery is an independent community tool and is not affiliated with or endorsed by the WARDOGS publisher. Estimates can become outdated after game changes; verify results in the game.
