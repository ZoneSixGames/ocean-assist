# Ocean Assist Technical Description

<h3><strong>Introduction:</strong></h3>

<p>Ocean Assist is developing a new micro-economic model that directs globalized resources into effective localized action in order to achieve measurable conservation results. The goal of the Ocean Assist platform is to develop a carbon offset ecosystem that uses machine learning to validate conservation actions and rewards XRP micro-payments using authentication with the </span></span></span><a href="https://xumm.app/" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>Xumm App</u></span></span></span></a>. Ocean Assist will implement a novel real-world use case of video action detection and classification to identify and reward sustainability actions using a decentralized geo-location XRPL network authorization. This value can be translated into authenticated carbon offset credits with Non-Fungible Token receipts registered on the XRPL, which will be sold to businesses in order to produce sustainable revenue for an action rewards XRP liquidity pool.</p>

<p><a href="https://arxiv.org/abs/2012.06567" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>A Comprehensive Study of Deep Video Action Recognition</u></span></span></span></a> is an excellent analysis of previous neural model implementations and research regarding action detection, upon which we have based our own implementation strategy. In particular, our aim is to utilize self-supervised learning to leverage a large amount of unlabeled input data by designing a pretext task to obtain free supervisory signals from data itself.</p>

<h4><strong>Desired objectives of ML Action Identification implementation for Ocean Assist:</strong></h4>

<ul>
	<li style="list-style-type:disc"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Identify waterfront scene or site landmarks</span></span></span></li>
	<li style="list-style-type:disc"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Identify clean up action&nbsp;</span></span></span></li>
	<li style="list-style-type:disc"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Object detection within clean up action sequence</span></span></span></li>
	<li style="list-style-type:disc"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Compare beginning and end or before and after of a scene to validate removal of object.</span></span></span></li>
</ul>

<h2><strong>Model Components</strong></h2>

<h3><strong>Sample Collection</strong></h3>

<ul>
	<li style="list-style-type:disc"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Collect sample video of before and after site cleaning as well as actions of cleaning that area (1000+ scenes of sample media recommended).</span></span></span></li>
	<li style="list-style-type:disc"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Create a standard to normalize user data collection including orientation of camera, resolution, distance from scene, area size, etc.</span></span></span></li>
</ul>

<p><strong>Dataset Preparation</strong></p>

<p><em>Notes on Video Data format for MMAction from</em><a href="http://www.openmmlab.com" style="text-decoration:none"><span style="font-size:13pt"><span style="font-family:Arial"><span style="color:#000000"><strong><em> </em></strong></span></span></span><span style="font-size:13pt"><span style="font-family:Arial"><span style="color:#1155cc"><strong><em><u>OpenMMLab</u></em></strong></span></span></span></a></p>

<p>MMAction supports two types of data format: raw frames and video. The former is widely used in previous projects such as <a href="https://github.com/yjxiong/temporal-segment-networks" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>TSN</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">. This is fast (especially when SSD is available) but fails to scale to the fast-growing datasets. (For example, the newest edition of </span></span></span><a href="https://deepmind.com/research/open-source/open-source-datasets/kinetics/" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>Kinetics</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000"> has 650K videos and the total frames will take up several TBs.) The latter saves much space but is slower due to video decoding at execution time. To alleviate this issue, we use </span></span></span><a href="https://github.com/zhreshold/decord" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>decord</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000"> for efficient video loading.</span></span></span></p>

<p>For action recognition, both formats are supported. For temporal action detection and spatial-temporal action detection, we still recommend the format of raw frames.</p>

<p><strong>Supported datasets</strong></p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">The supported datasets are listed below. We provide shell scripts for data preparation under the path $MMACTION/data_tools/. To ease usage, we provide tutorials of data deployment for each dataset.</span></span></span></p>

<ul>
	<li style="list-style-type:disc"><a href="http://serre-lab.clps.brown.edu/resource/hmdb-a-large-human-motion-database/" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>HMDB51</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">: See </span></span></span><a href="https://github.com/open-mmlab/mmaction/tree/master/data_tools/hmdb51/PREPARING_HMDB51.md" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>PREPARING_HMDB51.md</u></span></span></span></a></li>
	<li style="list-style-type:disc"><a href="https://www.crcv.ucf.edu/data/UCF101.php" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>UCF101</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">: See </span></span></span><a href="https://github.com/open-mmlab/mmaction/tree/master/data_tools/ucf101/PREPARING_UCF101.md" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>PREPARING_UCF101.md</u></span></span></span></a></li>
	<li style="list-style-type:disc"><a href="https://deepmind.com/research/open-source/open-source-datasets/kinetics/" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>Kinetics400</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">: See </span></span></span><a href="https://github.com/open-mmlab/mmaction/tree/master/data_tools/kinetics400/PREPARING_KINETICS400.md" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>PREPARING_KINETICS400.md</u></span></span></span></a></li>
	<li style="list-style-type:disc"><a href="https://www.crcv.ucf.edu/THUMOS14/download.html" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>THUMOS14</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">: See </span></span></span><a href="https://github.com/open-mmlab/mmaction/tree/master/data_tools/thumos14/PREPARING_TH14.md" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>PREPARING_TH14.md</u></span></span></span></a></li>
	<li style="list-style-type:disc"><a href="https://research.google.com/ava/" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>AVA</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">: See </span></span></span><a href="https://github.com/open-mmlab/mmaction/tree/master/data_tools/ava/PREPARING_AVA.md" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>PREPARING_AVA.md</u></span></span></span></a></li>
</ul>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">You can switch to </span></span></span><a href="https://github.com/open-mmlab/mmaction/tree/master/GETTING_STARTED.md" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>GETTING_STARTED.md</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000"> to train and test the model.</span></span></span></p>

<p><span style="font-size:12pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Prepare annotations</strong></span></span></span></p>

<p><span style="font-size:12pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Prepare videos</strong></span></span></span></p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Please refer to the official website and/or the official script to prepare the videos. Note that the videos should be arranged in either (1) a two-level directory organized by ${CLASS_NAME}/${VIDEO_ID} or (2) a single-level directory. It is recommended using (1) for action recognition datasets (such as UCF101 and Kinetics) and using (2) for action detection datasets or those with multiple annotations per video (such as THUMOS14 and AVA).</span></span></span></p>

<h3><span style="font-size:12pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Extract frames</strong></span></span></span></h3>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">To extract frames (optical flow, to be specific), </span></span></span><a href="https://github.com/yjxiong/dense_flow" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>dense_flow</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000"> is needed.&nbsp;&nbsp;</span></span></span></p>

<h3><span style="font-size:18pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Model Training</strong></span></span></span></h3>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Ocean Assist will experiment with a couple of different pre-trained action detection models. We will be evaluating their accuracy of desired action prediction alongside latency and throughput speeds. Training models is expensive, so we will evaluate the cost effectiveness for project feasibility before running sample data on these models.</span></span></span></p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Pre-trained models include:</span></span></span></p>

<p><a href="https://github.com/ZoneSixGames/mmaction2" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>MMAction2</u></span></span></span></a></p>

<p><a href="https://github.com/sallymmx/ActionCLIP" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>ActionCLIP</u></span></span></span></a></p>

<p><a href="https://cv.gluon.ai/model_zoo/action_recognition.html#something-something-v2-dataset" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>Something-to-Something-V2</u></span></span></span></a></p>

<p><a href="https://cv.gluon.ai/model_zoo/action_recognition.html#kinetics400-dataset" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>Kinetics 400&nbsp;</u></span></span></span></a></p>

<p><a href="https://cv.gluon.ai/model_zoo/action_recognition.html#kinetics700-dataset" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>Kinetics 700</u></span></span></span></a></p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">The Ocean Assist platform model will run on either a GPU cluster cloud infrastructure or we will purchase our own NVIDIA GPU hardware outright, whichever is more economically viable for prototyping and production. Options for infrastructure deployment include:</span></span></span></p>

<ul>
	<li style="list-style-type:disc"><a href="https://www.nvidia.com/en-us/data-center/h100cnx/" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>NVIDIA H100CNX</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000"> - For real time action verification (production use)</span></span></span></li>
	<li style="list-style-type:disc"><a href="https://lambdalabs.com/service/gpu-cloud#pricing" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>Lambda Labs Cloud GPU Services &amp; hardware</u></span></span></span></a></li>
	<li style="list-style-type:disc"><a href="https://cloud.google.com/" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>Google Computing Services</u></span></span></span></a></li>
	<li style="list-style-type:disc"><a href="https://www.microsoft.com/en-us/ai/ai-platform" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>Microsoft Azure</u></span></span></span></a></li>
</ul>

<p>&nbsp;</p>

<h2><span style="font-size:22pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Application Framework:</strong></span></span></span></h2>

<h3><span style="font-size:16pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Geo-location based XRPL authentication</strong></span></span></span></h3>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">In order to prevent spamming or exploitation of the rewards platform, users will need to authenticate their account using Xumm wallet sign in and pin the geo-location of a clean-up action. The user will not be able to submit another clean-up event in that same location (or within a certain distance range) until a minimum amount of time has passed. Here is </span></span></span><a href="https://github.com/BlockLagoon/FieldBoss" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>example code for a geo-location based XRPL NFT minter</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000"> by BlockLagoon.</span></span></span></p>

<h3><span style="font-size:16pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Dashboard for tracking data and rewards</strong></span></span></span></h3>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">A simple graphic user interface to track environmental footprint, clean-up actions, estimated carbon offset, rewards points, and XRP payouts.</span></span></span></p>

<h3><span style="font-size:16pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Data visualization of global clean up efforts</strong></span></span></span></h3>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">A curated user experience made using </span></span></span><a href="https://reactjs.org/" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>React </u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">and </span></span></span><a href="https://threejs.org/" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>Three.js</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">, designed to incentivize healthy gamified competition amongst different areas, groups, individuals, or countries! Challenge a friend to a clean-up competition from different parts of the world.&nbsp;</span></span></span></p>

<h3><span style="font-size:16pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Peer-to-peer XRP token donation</strong></span></span></span></h3>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">Feeling generous? Reward others for their efforts directly with a simple XRP donation function. Example embeddable button code by </span></span></span><span style="font-size:10.5pt"><span style="font-family:Arial"><span style="color:#000000"><span style="background-color:#f6f8fa">Evan Schwartz</span></span></span></span><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000"> here:&nbsp; </span></span></span><a href="https://github.com/emschwartz/ripple-donate-widget" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>https://github.com/emschwartz/ripple-donate-widget</u></span></span></span></a></p>

<h3><span style="font-size:16pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Rewards Calculator &amp; XRP Liquidity Pool</strong></span></span></span><span style="font-size:16pt"><span style="font-family:Arial"><span style="color:#000000">&nbsp;</span></span></span></h3>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">The Rewards Calculator will be an automated system (potentially using XRPL web hooks after the amendment is passed) that rewards XRP for accumulated action points. Action points will be awarded on a scale based on the action identified by the model. For instance, picking up trash on the beach or other waterway will earn more points than placing a cup in a recycling receptacle, but both would receive less points than planting a tree. The inclusion of different action parameters and relative weights in the point system will be determined by their estimated CO2 emission reduction. The action points can then be converted into XRP and transferred in micro-payments when they have accumulated to a minimum XRP value (to be determined).&nbsp;</span></span></span></p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">The action rewards XRP liquidity pool will implement the expanded multi-signature capabilities recently introduced to the XRPL. This will increase the security of rewards assets and create a decentralized authorization structure for future changes to the rewards system after it has been initiated.</span></span></span></p>

<h3><span style="font-size:16pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Environmental Footprint calculator</strong></span></span></span></h3>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">A calculator to estimate consumption and environmental footprint for individuals and businesses. Sample carbon calculator repositories include:</span></span></span></p>

<ul>
	<li style="list-style-type:disc"><a href="https://github.com/sarah926/carbonFootprint" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>https://github.com/sarah926/carbonFootprint</u></span></span></span></a></li>
	<li style="list-style-type:disc"><a href="https://github.com/lizroper0/fe-earthbff" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>https://github.com/lizroper0/fe-earthbff</u></span></span></span></a></li>
	<li style="list-style-type:disc"><a href="https://github.com/absambam/Carbon-Footprint-Calculator" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>https://github.com/absambam/Carbon-Footprint-Calculator</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000"> (travel footprint calculator)</span></span></span></li>
</ul>

<p>&nbsp;</p>

<h3><span style="font-size:16pt"><span style="font-family:Arial"><span style="color:#000000"><strong>Carbon Credit Marketplace</strong></span></span></span></h3>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">In order to provide sustainable rewards to the active conservationists in the ecosystem, Ocean Assist plans to offer verifiable carbon offset packages utilizing the issuance of Non-Fungible Tokens on the XRPL.</span></span></span></p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">More than actual emissions units can be traded and sold under the Kyoto Protocols emissions trading scheme.</span></span></span></p>

<p><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">The other units which may be transferred under the scheme, each equal to one tonne of CO2, may be in the form of:</span></span></span></p>

<ul>
	<li style="list-style-type:disc"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">A removal unit (RMU) on the basis of </span></span></span><a href="https://unfccc.int/land_use_and_climate_change/lulucf/items/1084.php" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>land use, land-use change and forestry (LULUCF)</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000"> activities such as reforestation</span></span></span></li>
	<li style="list-style-type:disc"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">An emission reduction unit (ERU) generated by a </span></span></span><a href="https://unfccc.int/kyoto_protocol/mechanisms/joint_implementation/items/1674.php" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>joint implementation</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000"> project</span></span></span></li>
	<li style="list-style-type:disc"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000">A certified emission reduction (CER) generated from a </span></span></span><a href="https://unfccc.int/process-and-meetings/the-kyoto-protocol/mechanisms-under-the-kyoto-protocol/the-clean-development-mechanism" style="text-decoration:none"><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#1155cc"><u>clean development mechanism</u></span></span></span></a><span style="font-size:11pt"><span style="font-family:Arial"><span style="color:#000000"> project activity</span></span></span></li>
</ul>
<p><a href="https://www.climateactionreserve.org/how/program-resources/program-manual/">The Climate Action Reserve </a> is an offsets program working to ensure integrity, transparency, and financial value in the North American carbon market. It does this by establishing regulatory quality standards for the development, quantification, and verification of GHG emission reduction projects in North America; issuing carbon offset credits known as Climate Reserve Tonnes (CRTs) generated from such projects; and tracking the transaction of credits over time in a transparent, publicly-accessible system. Adherence to the Reserve’s high standards ensures that emission reductions associated with projects are real, permanent, and additional, thereby instilling confidence in the environmental benefit, credibility, and efficiency of the U.S. carbon market. Ocean Assist will register with the Climate Action Reserve and form the rewards system around the platforms ability to prove compliance with different reduction standards evaluated by the organisation.</p>

