<!--
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->
{% include JB/setup %}
<div class="tech_deploy">
  <div class="container">
    <h2>Technologies</h2>
    <div class="border row">
      <div class="border col-md-4 col-sm-4" style="height:200px;">
        <div class="panel-content">
          <label class="_default-text" style="margin-top: 20px;">
            <img src="./assets/themes/zeppelin/img/yarn-k8s.png" width="290px">
          </label>
          <label class="_hover-text">
            <div style="min-height:140px; padding: 20px 10px 10px 10px;">
              Submarine supports Yarn, Kubernetes, Docker with Resource Scheduling.
            </div>
            <a href="/docs/0.7.2/interpreter/spark.html" class="panel-button">USE NOW <span class="glyphicon glyphicon-chevron-right"></span></a>
          </label>                     
        </div>
      </div>
       <div class="border col-md-4 col-sm-4" style="height:200px;">
         <div class="panel-content">
           <label class="_default-text" style="margin-top: 20px;">
             <img src="./assets/themes/zeppelin/img/tf-pytorch.png" width="290px">
           </label>
           <label class="_hover-text">
             <div style="min-height:140px; padding: 20px 10px 10px 10px;">
               Submarine supports Python, Tensorflow, PyTorch with machine learning algorithm development.
             </div>
             <a href="/docs/0.7.2/interpreter/spark.html" class="panel-button">USE NOW <span class="glyphicon glyphicon-chevron-right"></span></a>
           </label>
         </div>
       </div>
      <div class="border col-md-4 col-sm-4" style="height:200px;">
        <div class="panel-content">
          <label class="_default-text" style="margin-top:20px;">
            <img src="./assets/themes/zeppelin/img/spark-flink.png" width="290px">
          </label>
          <label class="_hover-text">
            <div style="min-height:140px; padding: 10px;">
              Submarine workbench supports Spark, Flink with data processing and model prediction processing.
            </div>
            <a href="/docs/0.7.2/interpreter/python.html" class="panel-button">USE NOW <span class="glyphicon glyphicon-chevron-right"></span></a>
          </label>
        </div>
      </div>
    </div>
    <div class="col-md-12 col-sm-12 col-xs-12 text-center">
      <p class="bottom-text">
        See more details in Submarine more feature.
        <a href="/docs/0.8.0-SNAPSHOT/manual/interpreters.html">LEARN MORE <span class="glyphicon glyphicon-chevron-right" style="font-size:15px;"></span></a>
      </p>
    </div>    
    <hr />
    <div class="border row">
      <h2 style="padding-bottom: 8px;">Deployments</h2>
      <div class="border col-md-6 col-sm-6">
        <div class="panel-content-user">
          <label style="width: 100%;">
            <div style="position:relative;width:100%;text-align:center;">
              <span class="user-icon fa fa-user"></span>
              <span class="title-text">Single User</span>
            </div>
          </label>
          <label class="content-text">
            Local Spark, 6 Built-in visualizations, Display system, Dynamic form, Multiple backends are supported.<br/>
            <a href="/docs/0.7.2/install/install.html" class="user-button">LEARN MORE</a>
          </label>
        </div>
      </div>
      <div class="border col-md-6 col-sm-6">
        <div class="panel-content-user">
          <label style="width: 100%;">
            <div style="position:relative;width:100%;text-align:center;">
              <span class="user-icon fa fa-users"></span>
              <span class="title-text">Multi-User</span>
            </div>
          </label>
          <label class="content-text">
            Zeppelin supports Multi-user Support w/ LDAP. Let's configure Zeppelin for your yarn cluster.<br/>
            <a href="/docs/0.7.2/security/shiroauthentication.html" class="user-button">LEARN MORE</a>
          </label>                 
        </div>
      </div>
    </div>
  </div>
</div>
