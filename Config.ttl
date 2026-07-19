# =========================================================================
# Vivado Automated High-Density Implementation Script for Perfmem1
# =========================================================================

# Create project targeting the high-performance hardware family
create_project -force perfmem1_proto ./perfmem1_proto -part xc7vx690tffg1761-2

# Add VHDL top-level architecture source files
add_files ./rv32im_pipeline_core.vhd
# (Include additional paths to your ID_stage or CPU folders here if modular)

# Read the constraints profile for timing specifications
read_xdc ./design_constraints.xdc

# -------------------------------------------------------------------------
# Synthesis Optimization (Aggressive Logic Reduction Rules)
# -------------------------------------------------------------------------
# -flatten_hierarchy rebuilt: Optimizes logic structures across hierarchies
# -resource_sharing on: Forces structural mapping to minimize redundant gates
synth_design -top rv32im_pipeline_core -flatten_hierarchy rebuilt -resource_sharing on

# -------------------------------------------------------------------------
# Implementation Optimization (Performance and Placement Rules)
# -------------------------------------------------------------------------
# Optimizes multi-driven nets and long wire delays prior to placement
opt_design -directive Explore

# Performance_HighUtilLUTs: Instructs physical layout placement algorithms 
# to group deep combinational clusters into tightly clustered hardware slices
place_design -directive Performance_HighUtilLUTs

# Refines and balances routing matrices to minimize logic cascade latency
phys_opt_design -directive AggressiveExplore
route_design -directive Explore

# -------------------------------------------------------------------------
# Generate Verification Reports
# -------------------------------------------------------------------------
report_timing_summary -delay_type min_max -file ./timing_summary.txt
report_utilization -file ./utilization_report.txt
