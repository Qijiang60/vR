<article class="item-page">
	<h2>
							<a href="/articles/learn-vco/265-how-to-run-a-perl-script-from-a-vcenter-orchestrator-workflow.html"> How to run a Perl Script from a vCenter Orchestrator Workflow</a>
					</h2>
	
	<ul class="actions">
						<li class="print-icon">
			<a href="/articles/learn-vco/265-how-to-run-a-perl-script-from-a-vcenter-orchestrator-workflow.html?tmpl=component&amp;print=1&amp;page=" title="Print article < How to run a Perl Script from a vCenter Orchestrator Workflow >" onclick="window.open(this.href,'win2','status=no,toolbar=no,scrollbars=yes,titlebar=no,menubar=no,resizable=yes,width=640,height=480,directories=no,location=no'); return false;" rel="nofollow"><span class="icon-print"></span>Print</a>			</li>
		
					<li class="email-icon">
			<a href="/component/mailto/?tmpl=component&amp;template=rt_stratos&amp;link=157fdf86775a8bf5523125e2678aed336d41399b" title="Email this link to a friend" onclick="window.open(this.href,'win2','width=400,height=350,menubar=yes,resizable=yes'); return false;" rel="nofollow"><span class="icon-envelope"></span>Email</a>			</li>
						</ul>




	<dl class="article-info">
	<dt class="article-info-term">Details</dt>
	<dd class="category-name">
				Category: <a href="/articles/learn-vco.html">Learn VCO</a>		</dd>
	<dd class="published">
	Published: Monday, 22 July 2013 18:22	</dd>
	<dd class="createdby">
				
			Written by Burke Azbill		</dd>
	<dd class="hits">
	Hits: 11711	</dd>
	</dl>




             <div class="roksocialbuttons addthis_toolbox  "
                addthis:url="https://www.vcoteam.info:443/articles/learn-vco/265-how-to-run-a-perl-script-from-a-vcenter-orchestrator-workflow.html"
                addthis:title="How to run a Perl Script from a vCenter Orchestrator Workflow">
             <div class="custom_images"><a class="addthis_button_twitter"><span></span></a><a class="addthis_button_facebook"><span></span></a><a class="addthis_button_google"><span></span></a>
             </div>
             </div><p><img src="/images/perl-logo.jpg"        width="116" height="60"    alt="PERL Logo" title="PERL Logo"  class="multithumb"  style="float: left;"    />The goal of this tutorial is to create a simple workflow with a single string input that will be passed to a locally hosted Perl script for execution. The results of the script will be returned to vCO and stored in the workflow output. This could come in handy if you have existing systems that already have Perl based management scripts and you wish to incorporate their automation into your Orchestration policies.</p>
 
<p> </p>
<h2 class="StepTitle">Concepts Covered In this tutorial</h2>
<p>The following concepts will be covered:</p>
<ul>
<li>Enabling Local Process execution</li>
<li>Filesystem rights configuration</li>
<li>Use of the "command" object to execute a locally hosted PERL script</li>
</ul>
<h2 class="StepTitle">Tools Used</h2>
<p>The following tools are used for this tutorial:</p>
<ul>
<li>vCenter Orchestrator 5.1.1 Appliance (Perl is pre-installed in the appliance)</li>
<li>Simple Perl Script configured with at least 1 input parameter</li>
</ul>
<h2 class="StepTitle">vCenter Orchestrator Configuration</h2>
<p>vCO requires special configuration considerations when dealing with local commands and filesystem access. The default settings of a vCO server limit access to specific local folders and prevents the execution of local processes. Local processes include scripts and commands hosted on the vCO Server.</p>
<p><b>Configure vCO to allow execution of local processes:</b></p>
<ul>
<li>Go to /app-server/server/vmo/conf</li>
<li>Edit the vmo.properties file and add (if not already done) the following line to enable</li>
</ul>
<p>com.vmware.js.allow-local-process=true<br /> <b>Configure vCO to allow filesystem access to scripts folder:</b></p>
<ul>
<li>Go to /app-server/server/vmo/conf</li>
<li>Edit (or create if it does not yet exist) the js-io-rights.conf file</li>
<li>Assuming a folder name of "orchestrator" on an appliance, enter the following (adjust as desired for Windows):</li>
</ul>
<p>+rwx /orchestrator</p>
<ul>
<li>Add additional folders as desired. The line above adds (r)ead (w)rite e(x)ecute access to the /orchestrator folder to the vCenter Orchestrator Server service.</li>
</ul>
<p>Once the two files have been edited as described above, restart the vCenter Orchestrator Server service.</p>
<h2 class="StepTitle">Script Preparation</h2>
<p><img src="/images/stories/Script_Preparation.png"        width="458" height="105"    alt="Script_Preparation.png"   class="multithumb"      /> <br /><br /></p>
<p>Prepare a simple script as an initial test to confirm things are setup properly. Once a simple script such as the example here is working, you can then try out more complex scripts/programs.</p>
<ul>
<li>Create a new file named helloperl.pl in the <b>/orchestrator</b> folder</li>
<li>If using the appliance, be sure to set ownership on the file to the vco user/group: <b>chown vco.vco /orchestrator/helloperl.pl</b></li>
<li>Also make the file executable: <b>chmod 755 /orchestrator/helloperl.pl</b></li>
</ul>
<pre class="brush:perl">#!/usr/bin/perl
print "Hello, ", $ARGV[0];</pre>
<p>You now have a simple script and vCO configured with the ability to access and execute that script so it is time to create a workflow to do just that.</p>
<h2 class="StepTitle">Create Workflow</h2>
<p><img src="/images/stories/Create_Workflow.png"        width="189" height="61"    alt="Create_Workflow.png"   class="multithumb"      /> <br /><br /></p>
<p>Open the vCenter Orchestrator Client and create a new workflow called "Hello Perl"<br /> Please use this exact name as it will be the subject of my next tutorial (described in the Summary and Next Steps section at the bottom of this tutorial.)</p>
<h2 class="StepTitle">Add Scriptable Task</h2>
<p><img src="/images/stories/Add_Scriptable_Task.png"        width="540" height="267"    alt="Add_Scriptable_Task.png"   class="multithumb"      /> <br /><br /></p>
<ol>
<li>Add a single scriptable task to the workflow</li>
<li>Rename it from "Scriptable task" to something like "Run Script" if desired for better appearance</li>
</ol>
<p>When complete, it should look like the screenshot above.</p>
<h2 class="StepTitle">Create/Bind Inputs and Outputs</h2>
<p><img src="/images/stories/CreateBind_Inputs_and_Outputs.png"        width="540" height="619"    alt="CreateBind_Inputs_and_Outputs.png"   class="multithumb"      /> <br /><br /></p>
<p>Since this is a simple test workflow, it will take two inputs and produce two outputs. We'll prompt for the name of a script and another input for the parameters. For this example we will have a single paramter. For a more complex script, you could dynamically build up a number of parameters in elements in earlier parts of a custom workflow, storing them in a workflow attribute and then bind that attribute as an input to the "run script" scriptable task.</p>
<p><b>Click on the Scriptable task and add the following inputs:</b><br /> scriptName (string) - Create New Workflow Input Parameter<br /> scriptParams (string) - Create New Workflow Input Parameter</p>
<p><b>Add the following Output:</b><br /> scriptOutput (string) - Create New Workflow Output<br /> scriptResult (number) - Create New Workflow Output</p>
<p> </p>
<h2 class="StepTitle">Confirm Bindings</h2>
<p><img src="/images/stories/Confirm_Bindings.png"        width="540" height="329"    alt="Confirm_Bindings.png"   class="multithumb"      /> <br /><br /></p>
<p>Click on the "Visual Binding" tab of the "Run Script" to confirm the bindings are as shown in the above screenshot.</p>
<h2 class="StepTitle">Set Presentation</h2>
<p><img src="/images/stories/Set_Presentation.png"        width="540" height="383"    alt="Set_Presentation.png"   class="multithumb"      /> <br /><br /></p>
<ol>
<li>Click on the "Presentation" tab, then choose the "scriptName" input</li>
<li>Add a new property to the input for default value</li>
<li>Set the default value to "/orchestrator/helloperl.pl"</li>
<li>Add a new property to the input for "Mandatory" and set to "Yes"</li>
<li>Click "Save"</li>
</ol>
<h2 class="StepTitle">Code the Scriptable Task</h2>
<p>Insert the following code into the scriptable task:</p>
<pre class="brush:javascript">// Prepare Command line and parameters to execute:
cmd = scriptName + " " + scriptParams;
System.debug("executing cmd: " + cmd);

// Create and execute the command:
var command = new Command(cmd);
command.execute(true);

// Display command results and output 
var scriptResult = command.result;
System.debug("Script Result: " + scriptResult);

var scriptOutput = command.output;
System.debug("Script Output: " + scriptOutput);</pre>
<h2 class="StepTitle">How to use the output</h2>
<p>The results and output could be passed to another scriptable task for parsing and additional actions if desired. In that case, you would need to move the "scriptOutput" and "scriptResult" Output Parameters to Attributes.</p>
<h2 class="StepTitle">Save, Description, Version</h2>
<p><img src="/images/stories/Save__Description__Version.png"        width="540" height="386"    alt="Save__Description__Version.png"   class="multithumb"      /> <br /><br /></p>
<p>This completes the creation of the workflow so it should be saved.<br /> When saving a workflow, be sure to provide a nice description (1) so that it is obvious what the workflow is designed to perform. Here, we'll add the following:<br /> <i>This workflow will execute a simple Perl script that takes a single parameter. The output of the workflow should be the result code and any script output generated.</i></p>
<p>In addition to a description, you should version your work (and increment the version each time you update the workflow.) Go ahead and do that now (2). When you set the version, you'll be prompted for a version comment. I typically start with something like: Initial Version.</p>
<p>Save and Close the workflow now, we're ready to run it.</p>
<h2 class="StepTitle">Execute the Workflow using vCO Client</h2>
<p><img src="/images/stories/Execute_the_Workflow_using_vCO_Client.png"        width="540" height="430"    alt="Execute_the_Workflow_using_vCO_Client.png"   class="multithumb"      /> <br /><br /></p>
<p>At this point, you should still have the vCO Client open and the "Hello Perl" workflow selected.</p>
<ul>
<li>Right-Click on your workflow and select "Execute" (or use one of the other methods to start the workflow)</li>
<li>The "Script Name" should have the default value that was specified at design time</li>
<li>Enter your first name in the "Script Parameters" box</li>
<li>Click "Submit"</li>
</ul>
<h2 class="StepTitle">View Results: Variables</h2>
<p><img src="/images/stories/View_Results_Variables.png"        width="540" height="479"    alt="View_Results_Variables.png"   class="multithumb"      /> <br /><br /></p>
<p>After a successful run of the workflow, your results should appear similar to the above screenshot.</p>
<h2 class="StepTitle">View Results: Logs</h2>
<p><img src="/images/stories/View_Results_Logs.png"        width="540" height="142"    alt="View_Results_Logs.png"   class="multithumb"      /> <br /><br /></p>
<p>After a successful run of the workflow, your results should appear similar to the above screenshot.</p>
<h2 class="StepTitle">Summary and Next Steps</h2>
<p>This tutorial has provided you with a foundation for utilizing existing scripts and programs that may reside on your vCO server. Although this example was done on a vCO appliance, the same principals and steps apply to the Windows version of vCO. Leveraging existing Command Line utilities within workflows is an excellent integration example for systems that may not have a proper API available. <br /> <b>For example:</b><br /> Windows systems have .exe files such as the Active Directory tools dsadd.exe, dsmove.exe, dsquery.exe, etc...<br /> Some IPAM software, portals, or other misc systems may ship with a set of Perl, Python, or other management scripts</p>
<p>Any of the above may be utilized in the same way we have run the sample Perl script in this tutorial.</p>
<p>In my <a href="/articles/learn-vco/268-how-to-use-the-rest-api-to-start-a-workflow.html">next tutorial</a>, you'll learn how to access and run this "Hello Perl" workflow via the REST API !</p>
<ul>
<li>Locate the api documentation found on your vCO server</li>
<li>Find the workflow</li>
<li>Identify inputs, outputs, and the proper format</li>
<li>Work with XML based REST</li>
<li>Learn to force JSON for the input/output of REST Queries</li>
</ul><div class="attachmentsContainer">

<div class="attachmentsList" id="attachmentsList_com_content_default_265"></div>

</div>		
<ul class="pager pagenav">
	<li class="previous">
		<a href="/articles/learn-vco/266-making-vco-workflow-wrappers.html" rel="prev">
			<span class="icon-chevron-left"></span> Prev		</a>
	</li>
	<li class="next">
		<a href="/articles/learn-vco/261-get-supported-hardware-versions-from-cluster.html" rel="next">
			Next <span class="icon-chevron-right"></span>		</a>
	</li>
</ul>
