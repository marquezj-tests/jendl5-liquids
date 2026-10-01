jendl5-liquids: NCrystal data plugin
====================================

NCrystal data-only plugin (based on the `DummyDataPlugin` example in the NCrystal
repository, `examples/plugin_dataonly`) providing NCMAT files for the liquids of
the JENDL-5 thermal scattering sublibrary (TSL) evaluated at Kyoto University
(`KYOTO-U` in the ENDF-6 headers; author Y. Abe) plus heavy water (JAEA, Y. Abe co-author).

Materials (liquid states only; one file per material and temperature, named
`<material>_liquid_<T>K.ncmat`, referenced in NCrystal as `plugins::jendl5-liquids/<file>`):

| Material | ENDF-6 files (JENDL-5 TSL) | Evaluation |
|---|---|---|
| ethanol (C2H6O) | H(C2H6O)_0606, C(C2H6O)_0636, O(C2H6O)_0666 | KYOTO-U, FEB21 |
| benzene (C6H6) | H(C6H6)_0040, C(C6H6)_0640 | KYOTO-U, FEB21 |
| toluene (C7H8) | H(C7H8)_0605, C(C7H8)_0635 | KYOTO-U, FEB21 |
| mesitylene (C9H12) | H(C9H12)_0602, C(C9H12)_0632 | KYOTO-U, FEB21 |
| m-xylene (C8H10) | H(m-C8H10)_0609, C(m-C8H10)_0639 | KYOTO-U, FEB21 |
| methane (CH4) | H(CH4)_0033, C(CH4)_0633 | KYOTO-U, FEB21 |
| triphenylmethane (C19H16) | H(C19H16)_0614, C(C19H16)_0644 | KYOTO-U, FEB21 |
| heavy water (D2O) | D(D2O)_0011, O(D2O)_0051 | JAEA, SEP21 (A. Ichihara, Y. Abe (KU), K. Tada) |
| light water (H2O) | H(H2O)_0001, O(H2O)_0661 | KYOTO-U, DEC23 |

Heavy water is included although its evaluation is labelled JAEA, with Y. Abe (Kyoto
University) as co-author. Not included: the solid phases of the organic materials, and everything from other laboratories.

Notes and limitations
---------------------

* Files were generated with `NCMATComposer.set_dyninfo_scatknl` directly from the
  ENDF-6 MF7/MT4 data of each atom, combining the per-atom files of one molecule
  into one NCMAT file.
* Only inelastic scattering is present (no elastic component), as is appropriate
  for liquids, which have no MT2 in these evaluations.
* Densities are nominal values chosen by the converter, not taken from ENDF-6.
* ENDF-6 S(alpha,beta) grids are rescaled to NCMAT conventions (LAT=1 scaling,
  ln S converted to S).

Usage
-----

    pip install ./jendl5-liquids
    ncrystal-inspect plugins::jendl5-liquids/ethanol_liquid_300K.ncmat

The plugin directory is `src/ncrystal_plugin_jendl5-liquids/data/`. NCrystal discovers
installed plugins automatically through `ncrystal-pluginmanager`. Plugin data files are
only served on explicit request, hence the `plugins::` prefix (e.g. `ncrystal.load("plugins::jendl5-liquids/water_liquid_293.6K.ncmat")`).
Use `ncrystal-config --browse` or `NCrystal.browseFiles()` to list the files.
