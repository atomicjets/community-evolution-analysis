experiments-pci2013
===================
Code and data for paper: K. Konstantinidis, S. Papadopoulos and Y. Kompatsiaris, (2013) "Community Structure, Interaction and Evolution Analysis of Online Social Networks around Real-World Social Phenomena", PCI 2013, Thessaloniki, Greece, to appear.
###Abstract###
In recent years, Online Social Networks (OSNs) have been widely adopted by people around the globe as a means of real-time communication and opinion expression. As a result, most real-world events and phenomena are actively discussed online through OSNs such as Twitter and Facebook. However, the scale and variety of such discussions often hampers their objective analysis, e.g. by focusing on specific messages and ignoring the overall picture of a phenomenon. To this end, this paper presents an analysis framework to assist the study of trends, events and interactions performed between online communities. The framework utilizes an adaptive dynamic community detection technique based on the Louvain method [2] to study the evolution, overlap and cross-community dynamics in irregular, dynamically selected graph snapshots. We apply the proposed framework on a Twitter dataset collected by monitoring discussions around tweets containing extreme right political vocabulary, including messages around the Greek Golden Dawn party. The proposed analysis enables the extraction of new insights with respect to influential user accounts, topics of discussion and emerging trends, which could assist the work of journalists, social and political analysis scientists, but also highlights the limitations of existing analysis methods and poses new research questions.

In case you use this implementation in your research, please cite [1].

1. K. Konstandinidis, S. Papadopoulos, Y. Kompatsiaris. "Community Structure, Interaction and Evolution Analysis of Online Social Networks around Real-World Social Phenomena". In Proceedings of PCI 2013, Thessaloniki, Greece (to be presented in September).
2. V. Blondel, J.-L. Guillaume, R. Lambiotte, E. Lefebvre. "Fast unfolding of communities in large networks". In Journal of Statistical Mechanics: Theory and Experiment (10), P10008, 2008.

##Distribution Information##
This distribution contains the following:  
* a readme.txt file with instructions on how to use the different parts of the framework;
* a set of Python scripts (in the /python folder) that are used to parse the json files retrieved by the data collector in a "Matlab friendly" form.
* a set of Matlab scripts (in the /matlab folder) that are used to conduct the different network analysis steps described in [1];
* the set of data used in \[1] (anonymized due to Twitter terms of service).


##Data##
Due to Twitter's terms and permissions (https://dev.twitter.com/terms/api-terms), the data in the files regarding the paper has been transformed from a:  
_"authorname mentionedname1,mentionedname2,... time text"_  form, to a:  
_"authorID mentionedID1,mentionedID2,... time"_  form.  
So unfortunately the content of the tweets is not available.

##json Parsing (Python)##
The python code consists of 8 files containing user friendly scripts for parsing the required data from json files. There are 4 files to be used with jsons from any Twitter API dependant source.  
More specifically, they are used to create txt files which contain the mentions entries between twitter users as well as the time at which these mentions were made and the context in which they were included.  

The json_mention_141_multifile* files provide as many txt files as there are json files. 
They contain all the information required from the tweet in a readable form:

    user1 \TAB user2,user3... \TAB "created_at_timestamp" \TAB text \newline

The json_mention_matlab_singleFile* files provide a single file which contains only the data
required to perform the community analysis efficiently. They contain information in a "Matlab friendly" form:

    user1 \TAB user2 \TAB unix_timestamp \TAB \newline
    user1 \TAB user3 \TAB unix_timestamp \TAB \newline

This folder contains 6 files:

* <code>json\_mention\_multifile\_parser.py & json\_mention\_matlab\_singleFile_parser.py</code>  
    These files are used when the user has *.json files from another source.
* <code>json\_mention\_multifile\_noDialog\_parser.py & json\_mention\_matlab\_singleFile\_noDialog_parser.py</code>  
    These are similar to the previous files but the dataset folder path has to be inserted manually (Does not require the wxPython GUI toolkit).
* <code>txt\_mention\_matlab\_singleFile\_parser.py & txt\_mention\_matlab\_singleFile\_noDialog\_parser.py</code>  
    These files are used when the user has *.txt files from another source.  
    
##Evolution analysis (Matlab)##
Any new data to be analysed should be placed in the _../data/_ folder replacing the _1.txt_ file.  
The matlab code consists of 7 files which can either work as standalone scripts, or as functions of the _main.m_ script.  
The only thing that needs changing is a comment of the function line in each of the m-files (instructions are included in the m-files).  
These 7 files are:  
* <code>step1\_mentioning\_frequency.m</code>  
    This .m file extracts the user activity in respect to twitter mentions  
* <code>step2\_dyn_adj\_mat_wr.m</code>  
    This .m file extracts the dynamic adjacency matrices for each respective timeslot and save it into a mat format for use with the rest of the code but also in a csv gephi-ready format.  
* <code>step3\_comm\_detect_louvain.m</code>  
    This .m file detects the communities in each adjacency matrix as well as the sizes of the communities and the modularity for each timeslot using Louvain mentod (V. D. Blondel, J.-L. Guillaume, R. Lambiotte, and E. Lefebvre. Fast unfolding of communities in large networks. Journal of Statistical Mechanics: Theory and Experiment, 2008(10):P10008 (12pp), 2008.).  
* <code>step4\_comm\_evol_detect.m</code>  
    This .m file detects the evolution of the communities between timeslots.  
* <code>step5_commRoutes.m</code>  
    This .m file detects the routes created by the evolution of the communities between timeslots.  
* <code>step6\_commIndivCentrality.m</code>  
    This .m file extracts the user centrality of all adjacency matrices in between timeslots using the pagerank algorithm.  
* <code>step7\_commRouteAnal.m</code>  
    This .m file provides an analysis of the communities in respect to their evolution in terms of persistence, stability and user centrality.

There are also 4 assistive functions which are used to extract the position of each user in the adjacency matrix (_pos\_aloc\_of\_usrs.m_), to create the adjacency matrix (_adj\_mat\_creator.m_), to perform the community detection (_comm\_detect\_louvain.m_) and to extract the centrality of each user using the pagerank algorithm (_mypagerank.m_).

##Results##
The final outcome is a cell array in ../data/mats/signifComms.mat containing the most significant dynamic communities, their users and the centrality of the users.
_.../data/mats/commEvol.mat_ and _.../data/mats/commEvolSize.mat_ are also useful as they present the evolution of the communities and the community sizes respectfully.
A heatmap presenting the evolution and size of all evolving communities is also produced giving the user an idea of the bigger picture.
