# GFP_PUNCTA_INFO
#
import pandas as pd
#
import glob
#
csv_files_path = '/Users/noel/Desktop/GFPCSV/*.csv'
#
all_csv_files = glob.glob(csv_files_path);
#
GFPsurfaceSUM_results = []
#
GFPsurfaceAVG_results = []
#
GFPvolumeSUM_results = []
#
GFPvolumeAVG_results = []
#
for csv_file in all_csv_files:
    df = pd.read_csv(csv_file)
    
    try:
        GFPvolumeSUM_results.append(df['Volume (micron^3)'].sum())
        GFPvolumeAVG_results.append(df['Volume (micron^3)'].mean())
        GFPsurfaceSUM_results.append(df['Surface (micron^2)'].sum())
        GFPsurfaceAVG_results.append(df['Surface (micron^2)'].mean())
    except KeyError:
        print(f"Skipping file {csv_file} due to missing columns.")
        GFPvolumeSUM_results.append(None)
        GFPvolumeAVG_results.append(None)
        GFPsurfaceSUM_results.append(None)
        GFPsurfaceAVG_results.append(None)

results_df = pd.DataFrame({
    'FileName': all_csv_files,  
    'GFPvolumeSumResults': GFPvolumeSUM_results,
    'GFPvolumeAvgResults': GFPvolumeAVG_results,
    'GFPsurfaceSumResults': GFPsurfaceSUM_results,
    'GFPsurfaceAvgResults': GFPsurfaceAVG_results
})

# Ensure the directory exists before saving
output_dir = '/Users/noel/Desktop'
output_file = f"{output_dir}/GFPpunctaResults.csv"
results_df.to_csv(output_file, index=False)

print(f"Processing completed. Results saved to {output_file}")
