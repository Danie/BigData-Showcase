# Set parallel tasks
set parallelism     => "5";

# Set the required modules
require Rex::Lang::Java;
require Rex::Framework::Cloudera::PkgRepository;
require Rex::Framework::Cloudera::Hadoop::Configure;
require Rex::Framework::Cloudera::Hadoop::DataNode;
require Rex::Framework::Cloudera::Hadoop::HistoryServer;
require Rex::Framework::Cloudera::Hadoop::JobTracker;
require Rex::Framework::Cloudera::Hadoop::NameNode;
require Rex::Framework::Cloudera::Hadoop::TaskTracker;

#
# TASK: install_pseudo_cluster_cdh4_mrv1
#
task install_pseudo_cluster_cdh4_mrv1 => sub {

   # Install Java
   Rex::Lang::Java::setup({
      jse_version => "6",
      jse_type    => "jdk",
   });

   # Add the Cloudera Repository
   Rex::Framework::Cloudera::PkgRepository::setup({
      cdh_version => "cdh4",
   });

   # Create Hadop DataNode
   Rex::Framework::Cloudera::Hadoop::DataNode::setup();

   # Create Hadoop TaskTracker
   Rex::Framework::Cloudera::Hadoop::TaskTracker::setup({
      mr_version => "mrv1",
   });

   # Create Hadoop Primary NameNode
   Rex::Framework::Cloudera::Hadoop::NameNode::setup({
      namenode_role => "primary",
   });

   # Create Hadoop Secondary NameNode
   Rex::Framework::Cloudera::Hadoop::NameNode::setup({
      namenode_role => "secondary",
   });

   # Create Hadoop JobTracker
   Rex::Framework::Cloudera::Hadoop::JobTracker::setup({
      mr_version => "mrv1",
   });

   # Configure Hadoop Pseudo Cluster
   Rex::Framework::Cloudera::Hadoop::Configure::pseudo_cluster({
      mr_version => "mrv1",
   });

   # Format Hadoop NameNode
   Rex::Framework::Cloudera::Hadoop::NameNode::format_hdfs();

   # Start Hadoop HDFS-Services
   Rex::Framework::Cloudera::Hadoop::DataNode::start();
   Rex::Framework::Cloudera::Hadoop::NameNode::start();
   
   # Initialize Hadoop HDFS Directory-Structer
   Rex::Framework::Cloudera::Hadoop::NameNode::initialize_hdfs({
      mr_version => "mrv1",
   });

   # Add Users to Hadoop HDFS
   Rex::Framework::Cloudera::Hadoop::NameNode::create_user({
      user => "inovex",
   });

   # Start Hadoop JobTracker and TaskTracker
   Rex::Framework::Cloudera::Hadoop::JobTracker::start();
   Rex::Framework::Cloudera::Hadoop::TaskTracker::start();

};
