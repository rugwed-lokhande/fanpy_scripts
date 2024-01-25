# fanpy_scripts
Various steps for running fanpy calculations


---

**Directory Structure:**

- First level groups systems (database names).
- Second level represents molecular geometries.
- Third level corresponds to basis sets.
- Fourth level is for calculation methods with different settings.
- Fifth level contains individual calculations for each system at the specified method and basis.

**Setup:**

1. Load the conda environment with `module load conda`.
2. Activate the virtual environment with `conda activate /home/kimt1/envs/fanpy`.
3. Optionally, you can add these commands to your `.bashrc` for convenience.

**Steps:**

1. Create a new database directory within your chosen base directory.

2. For each geometry, make a directory and include an ".xyz2" file containing atom coordinates (in atomic units).

3. Create a directory for each basis set. Include a ".gbs" file obtained from [basissetexchange.org](https://www.basissetexchange.org/) (Format: Gaussian).

4. For HF calculations, create a directory and a ".com" file. You can use the provided Python module `run_calc` to generate these.

5. Run HF calculations. You can use the `run_calcs` function in `run_calc`. If needed, specify memory and time.

6. Generate fchk files from chk files using the `run_calcs` function.

7. Generate one and two-electron integrals as npy arrays using the `run_calcs` function.

8. Create directories for the wavefunction/method using the `make_wfn_dirs` function.

9. Generate scripts for running calculations. Define calculation details using the `write_wfn_py` function.

10. Run/submit the job. Use the `run_calcs` function with the appropriate paths.

11. To rerun calculations from a checkpoint, create a new script using `write_wfn_py` and run it.

12. If you need to modify calculations, edit the generated script to fit your requirements. You can use the `edit_file` function from the `edit_calculate` module.

---

Neccesary imports: 
`import run_calc   
  import os`   


**Examples:**

- **Step 5: Run HF calculations**

   - Example 1: Run HF calculations locally using `run_calc.run_calcs('paldus/*/sto-6g/hf/hf_sp.com')`.

   - Example 2: Submit jobs with specified time and memory using `run_calc.run_calcs('paldus/*/sto-6g/hf/hf_sp.com', time='1d', memory='3gb', outfile='results.out')`.

- **Step 6: Generate fchk from the chk files**

   - Use `run_calc.run_calcs('paldus/*/sto-6g/hf_sp.chk')` to generate fchk files.

- **Step 7: Generate one and two electron integrals as npy array**

   - Use `run_calc.run_calcs('paldus/*/sto-6g/hf_sp.fchk')` to generate integrals.

- **Step 8: Make directories for the wavefunction/method**

   - Create directories with `run_calc.make_wfn_dirs('paldus/*/sto-6g/', 'ap1rog', 10)`.

- **Step 9: Make scripts for running calculations**

   - Generate scripts with `run_calc.write_wfn_py('paldus/*/sto-6g/ap1rog/', 4, 'ap1rog', optimize_orbs=True, pspace_exc=[1, 2, 3, 4], objective='one_energy', solver='cma', ham_noise=1e-3, wfn_noise=1e-2, load_orbs=None, load_ham=None, load_wfn=None)`.

- **Step 10: Run/submit the job**

   - Submit jobs with `run_calc.run_calcs('paldus/*/sto-6g/ap1rog/calculate.py', time='1d', memory='3gb', outfile='results.out')`.

- **Step 11: Rerun calculations**

   - Rerun from a checkpoint with `run_calc.write_wfn_py('paldus/*/sto-6g/ap1rog/0/', 4, 'ap1rog', optimize_orbs=True, pspace_exc=[1, 2, 3, 4], objective='one_energy', solver='cma', ham_noise=1e-3, wfn_noise=1e-2, load_orbs=None, load_ham=None, load_wfn=None, load_chk='checkpoint.npy')` and then run with `run_calc.run_calcs('paldus/*/sto-6g/ap1rog/0/', time='1d', memory='3gb', outfile='results.out')`.

- **Step 12: Modify calculations**

   - Edit scripts manually or use `edit_calculate.edit_file` function.

---

Please adapt these examples to your specific use case and directory structure.
