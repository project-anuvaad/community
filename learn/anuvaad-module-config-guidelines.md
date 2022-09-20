# Anuvaad Module Config Guidelines



### Configs:

Parameters of a module that can be injected in and out of the system with zero to minimal code change in order to enable/disable/modify certain features of the system. The configs pertaining to modules of anuvaad data-flow pipeline can be broadly classified into 2 categories as:

1. Configs outside the build (docker image)
2. Configs within the build. (docker image)

### Configs outside the build:

These are those configs which are injected to the system on the fly, the changes injected thereby can be incorporated into the system on runtime or on just a restart of the system without having to re-build or push a logical piece of code. For instance, WFM reads configs for identifying different workflows configured. In order to add/edit/delete a workflow, one needs to make changes to the config file as required and push the file, the changes will be incorporated into the system on restart or through a reload API on runtime. (https://raw.githubusercontent.com/project-anuvaad/anuvaad/master/anuvaad-etl/anuvaad-workflow-mgr/config/etl-wf-manager-config-dev.yml) These files will be saved in the ‘configs’ folder outside the source code of the system.

### Configs within the build:

These configs travel with the build. Meaning, these configs will be a part of docker image. These configs can be controlled via an environment file during deployment or internally within the code. This also means that any changes in these parameters will need a rebuild and re-deployment of the system. However, no change in the logic or the code should be needed to incorporate the changes. Most of the hooks that are exposed for a given system fall under this category. These are mentioned in the ‘configs’ folder inside the source code. It is recommended to use just one \*\*\*config.py file inside the folder for all these configs for better maintainability. In case someone prefers to separate out config files based on concern, they can do so but bear the overhead of maintaining them. For convenience and readability, these configs are further divided as:

1. Cross module common configs
2. Module specific configs.

### Cross module common configs:

These configs are used across all modules. Configs like Kafka host, Mongo host, File Upload URL etc fall under this category as these will not change from module to module. However, if there’s a case where modules chose to use different values for these parameters they can go ahead by using a different variable. The point of having this is ensuring we don’t create redundant variables in the environment file and use them from the same variables that are already defined.

Eg: [https://github.com/project-anuvaad/anuvaad/blob/69b494224626d51a7baf0405603106a4a66a25c7/anuvaad-etl/anuvaad-extractor/aligner/etl-aligner/configs/alignerconfig.py#L3](https://github.com/project-anuvaad/anuvaad/blob/69b494224626d51a7baf0405603106a4a66a25c7/anuvaad-etl/anuvaad-extractor/aligner/etl-aligner/configs/alignerconfig.py#L3)

### Module specific configs:

These configs are specific to the module, and will change for each module. This category includes both the common variables (Eg: https://github.com/project-anuvaad/anuvaad/blob/69b494224626d51a7baf0405603106a4a66a25c7/anuvaad-etl/anuvaad-extractor/aligner/etl-aligner/configs/alignerconfig.py#L10 ) inside the code which are used at multiple places in your project and the variables deriving value from the environment file (Eg: [https://github.com/project-anuvaad/anuvaad/blob/69b494224626d51a7baf0405603106a4a66a25c7/anuvaad-etl/anuvaad-extractor/aligner/etl-aligner/configs/alignerconfig.py#L22](https://github.com/project-anuvaad/anuvaad/blob/69b494224626d51a7baf0405603106a4a66a25c7/anuvaad-etl/anuvaad-extractor/aligner/etl-aligner/configs/alignerconfig.py#L22) )

For convenience the second type of variables are categorised as:

1. Kafka Configs: Configs required for kafka like topics, consumer groups, partition keys etc. that are very specific to the module. Some other parameters required to customise your consumer and producer as per your requirement can be mentioned under this category.
2. Datastore Configs: Configs required for the datastore that is being used which is mostly Mongo in our use case. In case you’re using MySQL, Redis, Elasticsearch etc, mention the required parameters in this category.
3. Module Configs: All other configs required for your module can be mentioned here.

Note: It is recommended to have most of these parameters deriving values from the environment file only. In some cases, they can also be hard-coded within the code. It is mandatory for every file/class within the project to use these parameters from these variables of the \*\*\*config.py file only.

#### Having said that:

1. Please ensure the folder structure is perfectly maintained.
2. Never ever check-in sensitive data like AWS keys, passwords, PIIs etc in the config file, always erase/mask/encrypt them before pushing it to github.
3. You can check this file for reference: [https://raw.githubusercontent.com/project-anuvaad/anuvaad/wfmanager\_feature/anuvaad-etl/anuvaad-extractor/aligner/etl-aligner/configs/alignerconfig.py](https://raw.githubusercontent.com/project-anuvaad/anuvaad/wfmanager\_feature/anuvaad-etl/anuvaad-extractor/aligner/etl-aligner/configs/alignerconfig.py)
