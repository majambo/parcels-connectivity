# Fish Larvae Simulation Project

<div style="font-size: 16px;"> This project simulates the dispersal patterns of four different fish species larvae (Cryptobenthic, Parental, Resident, and Transient) 
in marine environments. The simulations are implemented using Parcels (Probably A Really Computationally Efficient Lagrangian Simulator), 
a set of Python classes and methods designed to create customizable particle tracking simulations using output from Ocean Circulation models.

The oceanographic data comes from WINDS-M (Western Indian Ocean Simulation, Multidecadal), a high-resolution (1/50° in the horizontal) 
regional ocean simulation spanning the SW Indian Ocean from 1993-2020, using the Coastal and Regional Ocean Community model (CROCO). 
WINDS-M is forced at the lateral boundaries by daily output from the 1/12° CMEMS (Copernicus Marine Environment Monitoring Service) 
GLORYS12V1 global ocean reanalysis and barotropic tides from TPXO9, at the surface hourly output from ERA5, and also includes climatological 
riverine fluxes.

Each simulation tracks larval movement from release at coral reef habitats through their pelagic larval duration (PLD) until potential settlement. 
The models account for species-specific parameters including egg type, PLD variations, and release timing patterns. Output data includes raw 
trajectory files, processed connectivity metrics, and visualizations that help analyze dispersal patterns, settlement success, and network 
connectivity between reef habitats.

</div>

## Project Structure
```bash
parcels/monthly
├───cryptobenthic/                 (Output directory for cryptobenthic species simulation)
│   ├───2010/                      (Raw simulation data for specific year)
│   ├───extracted/                 (Preprocessed and analyzed data)
│   └───animation/                 (Visualization outputs and animations)
├───parental/                      (Output directory for parental species simulation)
│   ├───2010/
│   ├───extracted/
│   └───animation/
├───resident/                      (Output directory for resident species simulation)
│   ├───2010/
│   ├───extracted/
│   └───animation/
├───transient/                     (Output directory for transient species simulation)
│   ├───2010/
│   ├───extracted/
│   └───animation/
├───simulation_models/             (Python simulation scripts)
│   ├───cryptobenthic_final.py     (Cryptobenthic species simulation model)
│   ├───parental_final.py          (Parental species simulation model)
│   ├───resident_final.py          (Resident species simulation model)
│   └───transient_final.py         (Transient species simulation model)
└───habitat_files/                 (Species-specific habitat data)
    ├───cryptobenthic_habitat.nc   (Cryptobenthic species habitat file)
    ├───parental_habitat.nc        (Parental species habitat file)
    ├───resident_habitat.nc        (Resident species habitat file)
    └───transient_habitat.nc       (Transient species habitat file)
```

## Model Descriptions

### Cryptobenthic Simulation (cryptobenthic_final.py)
- Simulates the dispersal patterns of cryptobenthic fish larvae
- Species Parameters:
  - Egg Type: Demersal
  - Mean PLD: 33.6 days
  - Standard Deviation PLD: 11.6 days
  - Maximum PLD: Mean PLD + 2*STDv PLD
  - Release Time: 6am daily
  - Release Period: January to December
- Creates output directory: `cryptobenthic/`
- Generates raw data in yearly subdirectories (e.g., `2010/`)
- Processes and stores analyzed data in `extracted/`
- Saves visualization outputs in `animation/`

### Parental Simulation (parental_final.py)
- Simulates the dispersal patterns of parental fish larvae
- Species Parameters:
  - Egg Type: Demersal
  - Mean PLD: 22.6 days
  - Standard Deviation PLD: 5.9 days
  - Maximum PLD: Mean PLD + STDv PLD
  - Release Time: 6am daily
  - Release Period: January to December
- Creates output directory: `parental/`
- Following same directory structure as cryptobenthic simulation

### Resident Simulation (resident_final.py)
- Simulates the dispersal patterns of resident fish larvae
- Species Parameters:
  - Egg Type: Pelagic
  - Mean PLD: 37.2 days
  - Standard Deviation PLD: 15.8 days
  - Maximum PLD: Mean PLD + STDv PLD
  - Release Time: 6am to 6pm
  - Release Period: January to December during new moon
- Creates output directory: `resident/`
- Following same directory structure as cryptobenthic simulation

### Transient Simulation (transient_final.py)
- Simulates the dispersal patterns of transient fish larvae
- Species Parameters:
  - Egg Type: Pelagic
  - Mean PLD: 58.6 days
  - Standard Deviation PLD: 17.6 days
  - Maximum PLD: Mean PLD + STDv PLD
  - Release Time: 6pm
  - Release Period: January to December during full moon
- Creates output directory: `transient/`
- Following same directory structure as cryptobenthic simulation

## Directory Contents

### Year Directories (e.g., 2010/)
- Contains raw simulation output data in .zarr format
- File naming convention: `{species}_release_{YYYYMM}.zarr`
  - Example: `parental_release_201001.zarr` to `parental_release_201012.zarr`
  - YYYY: Year (e.g., 2010)
  - MM: Month (01-12)

### Extracted Directory
- Contains preprocessed and analyzed data
- Generated using `preprocessing.ipynb` Jupyter notebook
- Contains four main output files:

1. `network_metrics_2010.parquet`
   - Network connectivity metrics calculated from the simulation
   - Format: Parquet file

2. `settlement_data_2010_combined.parquet`
   - Combined settlement data from all months (January to December)
   - Aggregated raw data from monthly simulations
   - Format: Parquet file

3. `settlement_data_2010_matched.parquet`
   - Combined settlement data with matched release and settlement nodes
   - Enhanced version of combined data with node relationships
   - Format: Parquet file

4. `raster_2010_metrics.tif`
   - Stacked raster containing multiple connectivity metrics:
     - Outflow
     - Inflow
     - Local retention
     - Retention percentage
     - Net flow
   - Format: GeoTIFF

### Animation Directory
- Contains visualization outputs
- Includes particle tracking animations
- Supported formats: [specify formats, e.g., GIF, MP4]

## Running the Models

To run a simulation for a specific species:

1. Navigate to the parcels monthly directory
2. Run the corresponding Python script:
```bash
python cryptobenthic_final.py  # For cryptobenthic species
python parental_final.py       # For parental species
python resident_final.py       # For resident species
python transient_final.py      # For transient species
```

## Output Directory Structure
Each simulation creates its own species-specific directory with the following structure:
```bash
species_name/
├───2010/           # Raw simulation data (.zarr files)
├───extracted/      # Processed data files
│   ├───network_metrics_2010.parquet        # Network connectivity metrics
│   ├───settlement_data_2010_combined.parquet    # Combined monthly settlement data
│   ├───settlement_data_2010_matched.parquet     # Matched release-settlement data
│   └───raster_2010_metrics.tif            # Stacked connectivity metrics raster
└───animation/      # Visualization files
```

## File Naming Convention
Raw simulation data files follow this pattern:
- Format: `{species}_release_{YYYYMM}.zarr`
- Example: `parental_release_201001.zarr`
  - species: cryptobenthic, parental, resident, or transient
  - YYYY: Four-digit year
  - MM: Two-digit month (01-12)

Habitat files:
- Format: `{species}_habitat.nc`
- Example: `parental_habitat.nc`
- NetCDF format containing species-specific habitat data
- Data Structure:
  - Gridded coral reef locations
  - Resolution: 0.02 degree grid cells
  - Serves dual purpose in the model:
    1. Release nodes: Starting points for larval dispersal
    2. Settlement nodes: Potential settlement locations for larvae
- Each species has its specific habitat distribution within the grid

## Dependencies
[List required Python packages and versions]

## Contact
[Add contact information for project maintainers]

## Additional Notes
- Each simulation script is independent and can be run separately
- Output directories are created automatically if they don't exist
- Raw data in year directories should not be modified manually
- Processed data in extracted/ can be used for further analysis
