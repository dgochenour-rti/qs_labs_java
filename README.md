QuickStart Hands-On Exercises
-----------------------------

This repo contains labs used in DDS QuickStart training, Java version. Before
any given lab can be compiled and run, rtiddsgen should be run on the \*.idl
file in each directory. When performing this step, be sure to set the following:
- Generation: Example Files = "<disable>"
- Generation: Type files = "update"
- Generation: Makefiles = "create"
- Language = "Java"

Lab 1. Out Of The Box

  - Create IDL, then generate code
  - Decrease the DataWriter wait time from 4s to 2s
  - Give the sample members some values, just so we aren't writing empty data
  - In the QoS XML, update publication_name and subscription_name with meaningful values (student names, etc.)

Lab 2. Deadline Qos

  - Set deadline to 1.0 sec. on DataReader
  - Discuss why not working. (QoS mismatch)
  - Fix Reader listener to note Qos Error
    - In the Java API documentation, search for QosPolicyId_t; here you can match "4" to "DDS_DEADLINE_QOS_POLICY_ID"
    - rti_connext_dds-5.3.1/doc/api/connext_dds/api_java/classcom_1_1rti_1_1dds_1_1infrastructure_1_1QosPolicyId__t.html
  - Fix Writer to offer Deadline of 500ms

Lab 3. Time Based Filter

  - Speed up writer to 10HZ.
  - Note that Reader is getting swamped.
  - Set Reader Time Based Filter Minimum separation to 1.0 sec (in Qos XML) and extend deadline to 1.5 Sec.
  - Note any possible Syncopation and discuss that.


Lab 4. Keys and defining constants in the IDL.

  - Update IDL to add //@key and set unique key value per student
  - Also, add a constant string to the IDL for topic name and make changes in pub & sub to use this string.
  - Restart pubs and subs and discuss output.

Lab 5. Ownership and fail rollover.

  - In QoS XML, set EXCLUSIVE_OWNERSHIP_QOS on both Reader and writer.
  - Set Strength to unique value for each student. (assuming unique key values from last lab)
  - Note Results.
  - Now have students all use the same key value (all writing to the same instance)
  - Start running from lowest strength to highest strength.
  - Kill apps from Highest to lowest strength.

  (If everyone is not connected to the same network, demonstrate Ownership using Shapes Demo on a
  single machine)

Lab 6. Loading a user-defined XML QoS profile

  - Copy USER_QOS_PROFILES.xml to a new file called MY_QOS_PROFILES.xml
  - In new new file, rename the library to "MyLibrary" and profile to "MyProfile"
  - remove is_default_qos="true" from MyProfile
  - change DW and DR back to SHARED_OWNERSHIP_QOS
  - change publisher and subscriber code to use custom qosProvider

Lab 7. Listeners and Waitsets

  - Modify the subscriber code so that the read is done via a waitset instead of the default listener
  - Note that the waitset is actually a better approach in terms of not blocking the DDS threads

Lab 8. Reliability.

  - remove base_name="BuiltinQosLibExp::Generic.StrictReliable" from QoS
  - remove TBF and deadline QoS from DW and DR (just to clean things up)
  - Set reliability to RELIABLE_RELIABILITY_QOS in Reader. (Remember that the 
      DataWriter uses RELIABLE_RELIABILITY_QOS by default.)
  - Slow down writer to 1HZ.
  - Discuss that the data is now coming in at 1Hz
  - Now set writer to wait for reliable acknowledgements.
  - Discuss why the writer speed drops.
  - Set up writer protocol to correct.

Lab 9. Late Joiner History.

  - Set Writer and Reader Durability to TRANSIENT_LOCAL_DURABILITY_QOS.
  - Set History to KEEP_LAST_HISTORY_QOS, writer depth 10. Vary by students.
  - Set Reader History to depth 15.
  - Add monitoring library support
    - wait for errors from statically linked libs, discuss, then fix that in the makefile
  - Start Writer then Reader apps one at a time and discuss late joiner results.

Lab 10. Content Filtered topic

  - modify publisher to change the value of name on every other send
    - run pub and sub to verify that you are seeing the expected results
  - Now modify subscriber code to add a cft
    - add code to create the parameters and Filter
    - set the filter name
    - change the reader instantiation to use ContentFilteredTopic instead of normal topic
  - open admin console to show that no new topic is shown, the filtering is logical but
      does not spawn a new entity
