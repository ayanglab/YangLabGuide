# <span style="color:#2E86C1 ">*HPC Usage Tutorial*</span>

<!-- ## <span style="color:#2E86C1 ">*JUST follow this tutorial step by step !*</span> -->

<br>

## <span style="color:#2E86C1 ">*HPC Issues*</span>
1. if you have any questions, please check the [official docs](https://icl-rcs-user-guide.readthedocs.io/en/latest/hpc/) first
2. if you can not handle the problem, then contact ICT and raise a ticket at [this link](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-support/contact-us/
)

---

## <span style="color:#2E86C1 ">*Prerequisites (command and shell)*</span>
1. If you are not familiar with `command line` and `Linux`, please check the [Tutorial](https://icl-rcs-user-guide.readthedocs.io/en/latest/support/using-linux/command-line/
) first.
2. When using HPC, you need to submit a job to the queue via shell scripts. If you are not familiar with `shell scripts`, please check the [Tutorial](https://icl-rcs-user-guide.readthedocs.io/en/latest/support/using-linux/shell-scripting/) first.


---
<br>

## <span style="color:#2E86C1 ">*Computational resources*</span>

*We have 6 services on HPC, namely*

|Service|Status|Usage suggestions|
| :--- | :---: | :---: |
|`CX1`|decommissioning|
|CX2|decommissioned|
|`CX3`|ready| Single node applications or GPU applications using single precision. |
|`CX3 Phase2`|ready| For AI work we recommend using CX3 Phase 2 as the L40s GPUs have a newer microarchitecture (Ada Lovelace) and higher FP32 so they tend to be faster fo AI/ML workloads. |
|`HX1`|ready| * Multi-node parallel applications, typically using MPI to communicate between the compute nodes. <br> * GPU accelerated scientific applications that require double precision. |
|HX2|2025 summer| |

More details about HPC specifications can be found [here](https://icl-rcs-user-guide.readthedocs.io/en/latest/hpc/cluster-specification/
).

---
<br>


## <span style="color:#2E86C1 ">*Login HPC*</span>

1. Visit [HPC](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-access/
) for more details about access application.
2. Ask Dr Guang Yang or PostDocs in our group for the HPC access permission.
3. Apply Unified Access or VPN in Imperial, please check [Unified Access](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/remote-access/unified-access/).
    * `Attention:` If you are in the office or using the Imperial network, you can skip this step.
4. Login to the HPC using the following commands:
    ```bash
    # CX1 CX3
    ssh username@login.hpc.imperial.ac.uk

    # CX3 Phase2
    ssh username@login.cx3.hpc.ic.ac.uk
    ssh username@login-b.cx3.hpc.ic.ac.uk # AMD EPYC 7742 64-Core Processor (Rome)
    ssh username@login-ai.cx3.hpc.ic.ac.uk # Intel Xeon Platinum 8358 (Ice Lake)
    ssh username@login-bi.cx3.hpc.ic.ac.uk # Intel Xeon Platinum 8358 (Ice Lake)

    # HX1
    ssh username@login.hx1.hpc.ic.ac.uk
    ```
    * `Attention:` Replace `username` with your own username in `ic.ac.uk` address.

* `!!!Attention!!!:` If you want to use `HX1`, you need to apply for the HPC access permission first. After you get the HPC permission, you need to visit [this link](https://servicemgt.service-now.com/ask?id=sc_cat_item&sys_id=cbf5a4281b9c79101533a8a4bd4bcbea&sysparm_category=52a4a8f21be62110557837b5464bcbd24) to apply for the `HX1` access permission.
---
<br>


## <span style="color:#2E86C1 ">*Storage*</span>

More details about storage can be found [here](https://icl-rcs-user-guide.readthedocs.io/en/latest/hpc/getting-started/data-management-on-hpc/).

[Research Data Store (RDS)](https://icl-rcs-user-guide.readthedocs.io/en/latest/rds/) in Imperial College:
* Every user has a `home` directory and an `ephemeral` directory. 
    * The `home` directory is backed up and has a quota of `930GB`.     
    * The `ephemeral` directory is not backed up and has a quota of `10TB`. Data in the `ephemeral` directory will be deleted after `30 days`.
---

### <span style="color:#2E86C1 ">*visit your data and codes on RDS*</span>
<u>Visit RDS from your own laptop</u>, open the Explorer (Windows) or Finder (MacOS) and type the following commands:
```bash
# Home directory
\\rds.imperial.ac.uk\rds\user\<yourusername>\home #(Windows)
smb://rds.imperial.ac.uk/rds/user/<yourusername>/home #(macOS and Linux)

# Ephemeral storage
\\rds.imperial.ac.uk\rds\user\<yourusername>\ephemeral #(Windows)
smb://rds.imperial.ac.uk/rds/user/<yourusername>/ephemeral #(macOS and Linux)
```

---
* `!!!Attention!!!:` Currently the RDS is not connected/mounted to HX1 in any way and you must use tools such as `sftp/scp/rsync` to transfer data from the RDS to HX1. This separation remains in place to ensure that file system load on HX1 does not affect the RDS and vice-versa.
---

Use the following command to check the storage usage:
```bash
# HX1
/usr/lpp/mmfs/bin/mmlsquota --block-size auto /dev/gpfs
```
---
<br>


## <span style="color:#2E86C1 ">*Data&Code Transfer Between Our Local Workstation/NAS and HPC*</span>

* use this command to transfer data&code between our local workstation/NAS and HPC:

    ``` python
    '''use command `pwd` to get the path
    '''
    rsync -rz --info=progress2 --partial SOURCE_PATH TARGET_PATH
    ```

---
<br>


## <span style="color:#2E86C1 ">*Environment on HX1*</span>

More details about the environment can be found [here](https://icl-rcs-user-guide.readthedocs.io/en/latest/hpc/pilot/hx1/).

`Miniconda` is recommended for managing your Python environment. 

Follow the steps below to install `Miniconda` and build your own environment:
```bash
# step 1
mkdir -p ~/miniconda3

# step 2
curl https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o ~/miniconda3/miniconda.sh

# step 3
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3

# step 4
eval "$(~/miniconda3/bin/conda shell.bash hook)"

# step 5
conda activate base
conda list
conda install packages_you_need
conda deactivate
```

---
<br>


## <span style="color:#2E86C1 ">*Job Submission*</span>

More details about job submission can be found [here](https://icl-rcs-user-guide.readthedocs.io/en/latest/hpc/pilot/hx1/).

HX1 uses the PBS Pro job scheduler.


### <span style="color:#2E86C1 ">*Preparing a submission script job_test.pbs*</span>

```bash
#!/bin/bash
#PBS -N job_test
#PBS -l walltime=00:30:00
#PBS -l select=1:ncpus=18:mem=200gb:ngpus=1:gpu_type=A100
#PBS -j oe
#PBS -o job_test.out

cd $PBS_O_WORKDIR
cat $PBS_NODEFILE
module purge
module load TensorFlow/2.11.0-foss-2022a-CUDA-11.7.0

python my_TF_code.py
```

### <span style="color:#2E86C1 ">*Submit the job*</span>

The `qsub` command can be used to submit jobs to the queue, but on HX1 you must specify the hx queue i.e.:
```bash
qsub -q hx job_test.pbs
```


