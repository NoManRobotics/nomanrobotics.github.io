---
layout: post
title:  "定制您的个人机械臂"
categories: [ 产品 ]
image: assets/images/1.jpg
---
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
    body {
        font-family: Arial, sans-serif;
        line-height: 1.6;
        color: #333;
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
    }
    .custom-option {
        background-color: #f8f9fa;
        border-radius: 8px;
        padding: 20px;
        margin-bottom: 20px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .custom-option h3 {
        color: #007bff;
        margin-bottom: 15px;
    }
    .custom-option ul {
        padding-left: 20px;
    }
    .custom-option li {
        margin-bottom: 10px;
    }
    #model-container {
        width: 100%;
        height: 400px;
        margin-top: 30px;
        background-color: #f0f0f0;
        overflow: hidden;
        position: relative;
    }
    .controls {
        position: absolute;
        bottom: 10px;
        right: 10px;
        background-color: rgba(255, 255, 255, 0.7);
        padding: 8px;
        border-radius: 5px;
        font-size: 10px;
    }
    .control-group {
        margin-bottom: 3px;
    }
    label {
        display: inline-block;
        width: 60px;
        font-size: 10px;
    }
    input[type="range"] {
        width: 80px;
    }
    #joint-color-dropdowns {
        position: absolute;
        top: 10px;
        left: 10px;
        background-color: rgba(255, 255, 255, 0.7);
        padding: 8px;
        border-radius: 5px;
        font-size: 10px;
    }
    #joint-color-dropdowns select {
        margin: 2px 0;
        width: 100%;
        font-size: 10px;
    }
    #joint-color-dropdowns h3 {
        font-size: 12px;
        margin: 0 0 5px 0;
    }
</style>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/loaders/STLLoader.js"></script>
<body>

  <p>如果您有自己的想法，我们可以为您定制独一无二的个人机械臂。我们提供以下定制选项:</p>

  <div class="custom-option">
      <h3>全定制，10000元 ~ 25000元</h3>
      <ul>
          <li>完全按照您的需求设计和制造</li>
          <li>包括外观、功能和材料的全面定制</li>
          <li>提供专属的技术支持和售后服务</li>
      </ul>
  </div>

  <div class="custom-option">
      <h3>半定制，<=10000元</h3>
      <ul>
          <li>在我们现有模型的基础上进行部分定制</li>
          <li>可以选择外观颜色和部分功能定制</li>
          <li>享受标准的技术支持和售后服务</li>
      </ul>
  </div>

  <div class="custom-option">
      <h3>颜色定制，额外49元</h3>
      <ul>
          <li>选择您喜欢的颜色来个性化您的机械臂</li>
          <li>适用于我们所有的标准型号</li>
          <li>不影响机械臂的功能和性能</li>
      </ul>
  </div>

  <div id="model-container">
      <div id="joint-color-dropdowns"></div>
      <div class="controls">
          <div class="control-group">
              <label for="rotateX">旋转 X:</label>
              <input type="range" id="rotateX" min="-180" max="180" value="0">
          </div>
          <div class="control-group">
              <label for="rotateY">旋转 Y:</label>
              <input type="range" id="rotateY" min="-180" max="180" value="0">
          </div>
          <div class="control-group">
              <label for="rotateZ">旋转 Z:</label>
              <input type="range" id="rotateZ" min="-180" max="180" value="0">
          </div>
      </div>
  </div>

  <div class="custom-option">
      <h3>为了赚钱我不择手段</h3>
      <ul>
          <li>更换我们官网的LOGO为你的头像</li>
          <li>更换我们发布的post背景为你的照片</li>
          <li>帮你发送一个post来歌颂你的美德/征婚</li>
          <li>议价，联系淘宝客服</li>
      </ul>
  </div>

  <script> 
      let scene, camera, renderer, robot;
      const container = document.getElementById('model-container');

      const colors = {
          white: 0xffffff,
          silver: 0xc0c0c0,
          pink: 0xffc0cb,
          green: 0x00a300,
          gray: 0x808080,
          orange: 0xffa500,
          yellow: 0xffff00,
          purple: 0xad5cad,
          red: 0xff0000,
          skin: 0xffdbac
      };

      function init() {
          scene = new THREE.Scene();
          
          const width = container.clientWidth;
          const height = container.clientHeight;
          
          camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
          
          renderer = new THREE.WebGLRenderer({ antialias: true });
          renderer.setSize(width, height);
          renderer.setClearColor(0xf0f0f0);
          container.appendChild(renderer.domElement);

          // 增加主光源的强度
          const mainLight = new THREE.DirectionalLight(0xffffff, 1.0);
          mainLight.position.set(1, 1, 1);
          scene.add(mainLight);

          // 增加环境光的强度
          const ambientLight = new THREE.AmbientLight(0x404040, 0.8);
          scene.add(ambientLight);

          // 添加一个额外的光源
          const fillLight = new THREE.DirectionalLight(0xffffff, 0.5);
          fillLight.position.set(-1, 0.5, -1);
          scene.add(fillLight);

          camera.position.set(-10, 12, 15);
          camera.lookAt(5, 5, -6);

          fetch('../../URDF_description/urdf/carbonara.urdf')
              .then(response => {
                  if (!response.ok) {
                      throw new Error(`HTTP error! status: ${response.status}`);
                  }
                  return response.text();
              })
              .then(urdfContent => {
                  parseSimpleURDF(urdfContent);
              })
              .catch(error => {
                  console.error('加载URDF文件时出错:', error);
              });

          createJointColorDropdowns();
          setupControls();
      }

      function parseSimpleURDF(urdfContent) {
          const parser = new DOMParser();
          const xmlDoc = parser.parseFromString(urdfContent, "text/xml");
          
          const links = xmlDoc.getElementsByTagName('link');

          robot = new THREE.Group();

          const loader = new THREE.STLLoader();
          let loadedMeshes = 0;

          const meshFiles = {
              'base_link': 'base_link.stl',
              'Joint1_1': 'Joint1_v3_1.stl',
              'Joint2_1': 'Joint2_v2_1.stl',
              'Joint3_1': 'Joint3_v3_1.stl',
              'Joint4_1': 'Joint4_v2_1.stl',
              'Left_jaw_1': 'left_jaw_1.stl',
              'Right_jaw_1': 'right_jaw_1.stl'
          };

          for (let i = 0; i < links.length; i++) {
              const link = links[i];
              const linkName = link.getAttribute('name');
              const meshFile = meshFiles[linkName];

              if (meshFile) {
                  const filename = `../../URDF_description/meshes/${meshFile}`;
                  
                  loader.load(filename, 
                      (geometry) => {
                          const material = new THREE.MeshPhongMaterial({ color: 0xcccccc });
                          const meshObject = new THREE.Mesh(geometry, material);
                          meshObject.name = linkName;
                          robot.add(meshObject);

                          loadedMeshes++;
                          if (loadedMeshes === Object.keys(meshFiles).length) {
                              scene.add(robot);
                              robot.position.set(0, 0, 0);
                              robot.scale.set(0.1, 0.1, 0.1);
                              robot.rotation.x = -Math.PI / 2;
                              animate();
                          }
                      },
                      (xhr) => {
                          console.log((xhr.loaded / xhr.total * 100) + '% loaded');
                      },
                      (error) => {
                          console.error(`加载mesh失败: ${filename}, 错误:`, error);
                      }
                  );
              }
          }
      }

      function createJointColorDropdowns() {
          const jointNames = ['base_link', 'Joint1_1', 'Joint2_1', 'Joint3_1', 'Joint4_1', 'Left_jaw_1', 'Right_jaw_1'];
          const dropdownContainer = document.getElementById('joint-color-dropdowns');
          dropdownContainer.innerHTML = '<h3>关节颜色设置</h3>';

          jointNames.forEach(jointName => {
              const dropdown = document.createElement('select');
              dropdown.id = `${jointName}-color`;
              dropdown.innerHTML = `<option value="">选择 ${jointName} 颜色</option>`;
              for (let color in colors) {
                  dropdown.innerHTML += `<option value="${color}">${color}</option>`;
              }
              dropdown.addEventListener('change', (event) => changeJointColor(jointName, event.target.value));

              const label = document.createElement('label');
              label.htmlFor = dropdown.id;
              label.textContent = `${jointName}: `;

              const container = document.createElement('div');
              container.appendChild(label);
              container.appendChild(dropdown);
              dropdownContainer.appendChild(container);
          });
      }

      function changeJointColor(jointName, colorName) {
          const part = robot.getObjectByName(jointName);
          if (part && colors[colorName]) {
              part.material.color.setHex(colors[colorName]);
          }
      }

      function setupControls() {
          const rotateX = document.getElementById('rotateX');
          const rotateY = document.getElementById('rotateY');
          const rotateZ = document.getElementById('rotateZ');

          rotateX.addEventListener('input', updateRotation);
          rotateY.addEventListener('input', updateRotation);
          rotateZ.addEventListener('input', updateRotation);
      }

      function updateRotation() {
          if (robot) {
              robot.rotation.x = THREE.MathUtils.degToRad(parseFloat(rotateX.value)) - Math.PI / 2;
              robot.rotation.y = THREE.MathUtils.degToRad(parseFloat(rotateY.value));
              robot.rotation.z = THREE.MathUtils.degToRad(parseFloat(rotateZ.value));
          }
      }

      function animate() {
          requestAnimationFrame(animate);
          renderer.render(scene, camera);
      }

      init();
  </script>
</body>
