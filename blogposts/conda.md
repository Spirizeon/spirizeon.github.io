![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715286115628/8035e669-9507-4d9f-b4ff-8daf04c96e5b.png?w=1600&h=840&fit=crop&crop=entropy&auto=compress,format&format=webp)
# 🐍🌱 Setting Up Miniconda: Your Essential Guide to Conda on Linux 
**Author**: Yash Mehrotra, **Design**: Varsha Maheshwari
> 🚀 **Embark on Efficient Resource Management with SLURM!** 


Welcome, fellow developers and researchers, to the realm of efficient resource management powered by SLURM! 🌟 If you've ever found yourself lost amidst the labyrinth of job steps and Python environment management, fret not! Today, I'm thrilled to be your guide as we demystify the process of setting up Miniconda on your SLURM cluster. 🐍🔧 With Miniconda at your disposal, you'll wield the ability to effortlessly create and manage tailored virtual environments, perfectly suited for your machine learning endeavors. Gone are the days of sweating over configuration complexities; let's streamline your workflow and unlock the true potential of your SLURM cluster.

Ready to embark on this journey? Let's dive in and empower your development experience like never before!

* Step 1: Download the Miniconda installation script using the command:
    
    ```bash
    curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    ```
    
* Step 2: Make the script executable:
    
    ```bash
    chmod +x Miniconda3-latest-Linux-x86_64.sh
    ```
    
* Step 3: Run the installation script and follow the prompts:
    
    ```bash
    ./Miniconda3-latest-Linux-x86_64.sh
    ```
    
* Step 4: Add Miniconda to your PATH by updating your .bashrc file:
    
    ```bash
    echo 'PATH="/path/to/miniconda3/bin:$PATH"' >> ~/.bashrc
    ```
    
    The path will be specific to your system configuration. Replace `/path/to/miniconda3/bin` with the actual path to the Miniconda3 bin directory on your machine.
    
* Step 5: Activate the changes:
    
    ```bash
    source ~/.bashrc
    ```
    
    Now that Miniconda is up and running, let's initialize conda and get ready to supercharge our development workflow! 🎉
    
* Now you need to type the following command to activate the conda in you system :
    
    ```bash
    conda init
    ```
    
    **After running the above command, please close the terminal and reopen a new one. Then, you'll be ready to use conda.**
    

## 🎉 **Congratulations! You're Ready to Roll**

With Miniconda in place, you have the power to create tailored virtual environments for each of your machine learning projects. This flexibility ensures optimal performance and reproducibility in your workflows. Happy coding! 💻✨

## **Additional Tips :**

* **Create Environments:** Use `conda create --name myenv` to create a new environment.
    
* **Activate Environment:** Activate an environment with `conda activate myenv`.
    
* **Install Packages:** Install packages with `conda install package_name`.
    
* **List Environments:** List all environments with `conda env list`.
    

**Explore the vast ecosystem of libraries and tools available through Miniconda to supercharge your development workflow.**
