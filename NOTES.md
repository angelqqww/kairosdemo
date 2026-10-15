# KAIROS kidney CAD master — prototype draft

## Scope and status
Editable multi-file left-kidney approximation, not a patient-specific model or a clinically validated anatomical reference. No software, LED electronics, or functional sensors are implemented. Internal counts, proportions, branch topology, and placements are illustrative placeholders requiring anatomical confirmation. This is not yet a fabrication-release or fluid-functional assembly.

## Logical component hierarchy
- ORGAN / Outer Structure: anteriorCapsule, posteriorCapsule (`capsule.kcl`). Separate engineering half-shells.
- ORGAN / Internal Structures: five representative medullary pyramids; five minor calyces; two collecting branches; renal pelvis; renal artery and vein; ureter bend and proximal ureter. Individual part files and assembly occurrences are retained.
- ORGAN / Functional Regions: r01SuperiorCortex, r02UpperHilarCortex, r03LowerHilarCortex, r04InferiorCortex. These are axial visualization bands, NOT anatomical vascular segments or validated physiological territories.
- ORGAN / Sensor Interfaces: s01SensorCarrier, s02SensorCarrier, s03SensorCarrier. Separate slotted annular packaging placeholders, not embedded sensors.
- ORGAN / Sensor Reference Points: s01ReferencePoint, s02ReferencePoint, s03ReferencePoint; metadata in `sensorReferences.kcl`.
- ORGAN / Mounting Geometry: benchBase, organCradle. No shell fasteners or carrier retention mechanisms yet.
- ORGAN / Reference Geometry: origin, section planes, source sketches and sensor frames. Markers are visualization aids, not physical sensor parts.

Hierarchy above is the logical mapping of named bodies/files; no exported scene-tree hierarchy has been tested.

## Coordinates
Units: millimeters. Right-handed frame: +X patient left, +Y posterior, +Z superior. The left kidney's medial hilum faces -X. Origin is the modeled envelope's mid-height section reference at [0,0,0], not its center of mass or a registered patient landmark. Pole planes are z=-60 and +60 at the nominal length. No patient registration or natural renal tilt is asserted.

`main.kcl` owns assembly placements. `parameters.kcl`: reviewOffset=0 is assembled; 75 mm translates the anterior shell/sensor references anteriorly and cortex regions posteriorly for exploded review. The recorded sensor coordinates always refer to the assembled frame. Do not use review translations as physiological coordinates.

## Dimension provenance
Source read: Chaudhary et al., *Parameters Affecting the Kidney Size in Individuals without Known Renal Pathology: an Ultrasonographic Study*, PMCID PMC10427100, Introduction. It reports traditional textbook nominal size 12 x 6 x 3 cm and warns that actual measurements vary with population and individual characteristics. This supports a reference scale only, not the constructed surface or its internal anatomy.
Source address: `https://pmc.ncbi.nlm.nih.gov/articles/PMC10427100/`

| Parameter | Value | Classification / limitation |
|---|---:|---|
| organLength | 120 mm | Literature-reported nominal reference; not patient measurement |
| organWidth | 60 mm | Nominal section-scale parameter, not verified finished bounding width |
| organDepth | 30 mm | Nominal section-scale parameter, not verified finished bounding depth |
| shellReduction | 2 mm | Engineering envelope reduction, NOT biological capsule thickness or constant normal wall thickness |
| tissueClearance | 0.4 mm | Engineering envelope allowance; not verified minimum clearance |
| cortexReduction | 6 mm | Placeholder envelope reduction, NOT validated cortical thickness |
| sensorDiameter | 8 mm | Placeholder package diameter awaiting selected sensor |
| sensorClearance | 0.3 mm | Radial packaging clearance assumption |
| sensorCarrierHeight | 3 mm | Engineering placeholder |
| reviewOffset | 0 mm | Assembled master; 75 mm for review only |

Other dimensions in part files, including all internal diameters, lengths, proportions and counts, are placeholders requiring confirmation. Base is 100 x 65 x 5 mm with four diameter-4.4-mm mounting holes and a cable notch; cradle is 44 x 32 x 20 mm. These are bench-fixture assumptions, not anatomical dimensions.

## Sensor reference contract
All frames have local X=[1,0,0], local Y=[0,0,1], outward interface normal=[0,-1,0]. Frames are right-handed. All positions are non-contact interface-center placeholders; they are NOT verified points on the curved organ surface and their normals are NOT measured tissue normals. All variables are proposed future channels, not implemented or physiologically validated measurements.

| ID | Position mm | Associated region | Proposed variable |
|---|---|---|---|
| S01 | [0,-18,36] | R01 superior anterior cortex | surfaceTemperature |
| S02 | [12,-18,12] | R02 upper hilar anterior cortex | opticalReflectanceProxy |
| S03 | [0,-18,-36] | R04 inferior anterior cortex | surfaceStrain |

Each carrier has nominal 12-mm OD, 8.6-mm clear bore and 3-mm axial depth, with a radial cable slot. The intended 8-mm sensor package remains absent. There is no validated tissue-contact seat, pressure control, fixation or strain transfer. R03 has no sensor assignment yet. Kairos state thresholds and LED-response mappings remain unassigned.

## Parametric strategy and sequence
1. Solver-constrained circular sections, lofted to a curved envelope; separately reduced envelope produces engineering shell cavity.
2. Cortex envelope regions and simplified medullary components.
3. Hollow calyces, pelvis, vessel stubs and swept ureter elbow using explicit lumen subtraction.
4. Independently identified visualization regions and sensor metadata.
5. Separate sensor seats, reference markers, base and cradle.
6. Part-level constraint validation, exploded review and assembled review.

Shared profile functions accept dimensions to avoid duplicated construction. Their sketches are code-editable but are not point-and-click editable inside function bodies. Envelope and region dimensions update parametrically; internal placements and sensor positions still include nominal literal dimensions. Resizing is NOT yet a globally coordinated anatomical resize: revise and revalidate these dependent dimensions before changing scale substantially.

## Validation and remaining engineering work
All 17 geometry part files executed individually with every reported sketch fully constrained and no execution errors. Imported sketches are not counted in the main assembly report, so the individual reports are the constraint evidence. Exploded assembly and selected part views checked shell opening, hollow pelvis and ureter elbow, carrier slot/bore, base through-holes and cradle pocket.

The model uses real subtractive cavities in individual parts. Collecting-system junctions have NOT been validated as a continuous leak-tight lumen; no flow test, interference audit, minimum-wall analysis, strength analysis, print-process qualification or mesh conversion has been performed. Simplified pyramids, calyces and branches are not a complete reconstruction. Natural asymmetry, renal columns, detailed branching and microanatomy are not resolved.

Before physical manufacture: confirm reference anatomy, sensor package and contact modality; design shell/carrier retention and support for internal components; verify tissue/interface clearances and all connected lumens; choose material/process and establish tolerances. Reference-marker solids must be excluded from fabrication exports.

## Future visualization handoff
Preserve individual named occurrences and R/S metadata; do not merge the organ into one mesh. Keep assembled coordinates in the master. Export/tessellate and validate names, units, orientation, normals, manifoldness and region associations as a separate task. No GLB/GLTF conversion or Blender/Three.js round-trip is claimed. No LED device geometry is included because no package or mounting specification was supplied.
