# caps_URDF
<robot name="differential_drive_robot" xmlns:xacro="http://www.ros.org/wiki/xacro">
<!-- body dimensions-->

<!--caps -->

<xacro:property name="body_link_x_dim" value="0.5"/><!--1-->
<xacro:property name="body_link_y_dim" value="0.5"/><!--0.6-->
<xacro:property name="body_link_z_dim" value="0.2"/><!--0.3-->

<!-- wheel dimensions-->
<xacro:property name="wheel_link_radius" value="0.17"/>
<xacro:property name="wheel_link_length" value="0.07"/>

<xacro:property name="wheel_link_z_location" value="-0.25"/>

<!--caster dimensions-->
<xacro:property name="caster_link_radius" value="0.13"/>
<xacro:property name="caster_link_length" value="0.07"/>

<!--Material density-->
<xacro:property name="wheel_density" value="201.0"/>

<!-- PI constant-->
<xacro:property name="pi_const" value="3.14159265"/>

<!-- Robot body and wheel mass-->

<xacro:property name="body_mass" value="30.0"/> <!--wheel_mass 대략1.13kg나옴-->
<xacro:property name="wheel_mass" value="${wheel_density*pi_const*wheel_link_radius*wheel_link_radius*wheel_link_length}"/>

<!-- Moment of inertia of the wheel-->
<xacro:property name="Iz_wheel" value="${0.5*wheel_mass*wheel_link_radius*wheel_link_radius}"/>
<xacro:property name="I_wheel" value="${(1.0/12.0)*wheel_mass*(3.0*wheel_link_radius*wheel_link_radius+wheel_link_length*wheel_link_length)}"/>

<!-- color -->

<material name="metallic_grey">
    <color rgba="0.5 0.5 0.5 1"/> <!-- 회색조의 금속을 나타내는 색상 -->
</material>

<material name="metallic_black">
    <color rgba="0.2 0.2 0.2 1"/> <!--캐스터 관절 -->
</material>

<material name="metallic_black_bar">
    <color rgba="0.7 0.7 0.7 1"/> <!--캐스터 관절 바 -->
</material>

<material name="metallic_black_bar_W">
    <color rgba="0.9 0.9 0.9 1"/> <!--캐스터 관절 - 바퀴 연결  -->
</material>

<material name="white">
    <color rgba = "0 0 0 1"/>
  </material>

<material name="coral">
    <color rgba = "0.99 0.55 0.3 1"/>
  </material>

<material name="blue">
    <color rgba = "0 0 0.8 1"/>
  </material>

  <material name="gray">
    <color rgba = "1 1 1 1"/>
  </material>

  <material name="blueviolet">
    <color rgba = "0.222 0.888 0.38 1"/>
  </material>

  <material name="changF">
    <color rgba = "0.4 0.4 0.4 1"/>
  </material>

  <material name="changS">
    <color rgba = "0.3 0.3 0.3 1"/>
  </material>



<!-- This macro defines the complete inertial section of the wheel-->
<!-- It is used later in the code -->
<xacro:macro name="inertia_wheel">
        <inertial>
        <origin rpy="0 0 0" xyz="0 0 0"/>
        <mass value="${wheel_mass}"/>
        <inertia ixx="${I_wheel}" ixy="0.0" ixz="0.0" iyy="${I_wheel}" iyz="0" izz="${Iz_wheel}"/>
        </inertial>
</xacro:macro>


<!-- over here we includethe file that defines extra Gazebo options and motion control driver-->
<xacro:include filename="$(find robot_model_pkg)/urdf/robot.gazebo"/>

<!-- ##########################################################-->
<!-- FROM HERE WE DEFINE LINKS, JPOINTS BY CHANG JUN LEE-->
<!-- ##########################################################-->


<!-- we need to have this footprint link otherwise Gazebo will complain-->
<link name="footprint">
</link>
<joint name="footprint_joint" type="fixed">
        <parent link="footprint"/>
        <child link="base_link"/>
</joint>

<!-- ##########################################################-->
<!-- START : BODY LINK OF THE ROBOT-->
<!-- ##########################################################-->

<link name="base_link">
        <visual>
                <geometry>
                        <box size="${body_link_x_dim} ${body_link_y_dim} ${body_link_z_dim-0.1}" />
                </geometry>
                <material name = "changF"/>
                <origin rpy="0 0 0" xyz="0 0 0" />
        </visual>



        <visual>
                <geometry>
                        <cylinder length="0.50" radius="0.03"/>
                </geometry>
                  <material name="metallic_grey"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->           <!--왼쪽 캐스터지지대 -->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전        각 사이드 바 5도:0.0872665        /-->   
                <origin xyz = "0 0.298 0.006" rpy = "0 1.396263 -0.0523599"/> <!--80도 회전,3도 회전-->
        </visual>

        <visual>
                <geometry>
                        <cylinder length="0.50" radius="0.03"/>
                </geometry>
                  <material name="metallic_grey"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전        오른쪽 캐스터 지지대    /-->
                <origin xyz = "0 -0.298 0.006" rpy = "0 1.396263 0.0523599"/> <!--80도 회전-->
        </visual>

<!--###################################left caster part ###########################################-->

        <visual>
                <geometry>
                        <cylinder length="0.08" radius="0.033"/>   <!--caster 관절 부분 -->
                </geometry>
                  <material name="metallic_black"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전/-->
                <origin xyz = "-0.235 0.310 -0.035" rpy = "0 0 0"/> 
        </visual>


        <visual>
                <geometry>
                        <cylinder length="0.12" radius="0.027"/>   <!--caster 관절 바 부분 -->
                </geometry>
                  <material name="metallic_black_bar"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전/-->
                <origin xyz = "-0.235 0.310 -0.035" rpy = "0 0 0"/> 
        </visual>

        <visual>
                <geometry>
                        <box size= "0.05 0.1 0.01"/>        <!--caster 관절 바 - 바퀴 연결 부분 -->
                </geometry>
                  <material name="metallic_black_bar_W"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전/-->
                <origin xyz = "-0.235 0.310 -0.1" rpy = "0 0 0"/> 
        </visual>


        <visual>
                <geometry>
                        <box size= "0.05 0.20 0.01"/>        <!--caster 관절 바 - 바퀴 연결 일자 겉 부분 -->
                </geometry>
                  <material name="metallic_black_bar_W"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전/-->
                <origin xyz = "-0.235 0.360 -0.20" rpy = "1.570795 0 0"/> 
        </visual>

        <visual>
                <geometry>
                        <box size= "0.05 0.20 0.01"/>        <!--caster 관절 바 - 바퀴 연결 일자 속 부분 -->
                </geometry>
                  <material name="metallic_black_bar_W"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전/-->
                <origin xyz = "-0.235 0.26 -0.20" rpy = "1.570795 0 0"/> 
        </visual>



<!--###################################Right caster part ###########################################-->

        <visual>
                <geometry>
                        <cylinder length="0.08" radius="0.033"/>   <!--caster 관절 부분 -->
                </geometry>
                  <material name="metallic_black"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전/-->
                <origin xyz = "-0.235 -0.310 -0.035" rpy = "0 0 0"/> 
        </visual>


        <visual>
                <geometry>
                        <cylinder length="0.12" radius="0.027"/>   <!--caster 관절 바 부분 -->
                </geometry>
                  <material name="metallic_black_bar"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전/-->
                <origin xyz = "-0.235 -0.310 -0.035" rpy = "0 0 0"/> 
        </visual>

        <visual>
                <geometry>
                        <box size= "0.05 0.1 0.01"/>        <!--caster 관절 바 - 바퀴 연결 부분 -->
                </geometry>
                  <material name="metallic_black_bar_W"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전/-->
                <origin xyz = "-0.235 -0.310 -0.1" rpy = "0 0 0"/> 
        </visual>

        <visual>
                <geometry>
                        <box size= "0.05 0.20 0.01"/>        <!--caster 관절 바 - 바퀴 연결 일자 밖 부분 -->
                </geometry>
                  <material name="metallic_black_bar_W"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전/-->
                <origin xyz = "-0.235 -0.360 -0.20" rpy = "1.570795 0 0"/> 
        </visual>

        <visual>
                <geometry>
                        <box size= "0.05 0.20 0.01"/>        <!--caster 관절 바 - 바퀴 연결 일자 속 부분 -->
                </geometry>
                  <material name="metallic_black_bar_W"/>
                 <!--origin xyz = "0 0.30 0" rpy = "0 1.570795 0"/-->
                 <!--origin rpy="0 1.22173 0" xyz="0 0 0" 70도 회전/-->
                <origin xyz = "-0.235 -0.26 -0.20" rpy = "1.570795 0 0"/> 
        </visual>

<!--################################### under base ###########################################-->
        <visual>
                <geometry>
                        <box size="${body_link_x_dim-0.1} ${body_link_y_dim/1.2} ${body_link_z_dim}" />
                </geometry>
                <material name = "metallic_black_bar_W"/>
                <origin rpy="0 0 0" xyz="0 0 -0.1" />
        </visual>


        <visual>                                        
                <geometry>                      <!--번호판 느낌-->
                        <box size="0.18 ${body_link_y_dim/1.2} ${body_link_z_dim/3}" />
                </geometry>
                <material name = "metallic_black_bar_W"/>
                <origin rpy="0 1.919862 0" xyz="0.19 0 -0.1" />
        </visual>

        <visual>                                        
                <geometry>                      <!--번호판 느낌-->
                        <box size="0.18 ${body_link_y_dim/1.2} ${body_link_z_dim/3}" />
                </geometry>
                <material name = "metallic_black_bar_W"/>
                <origin rpy="0 -1.919862 0" xyz="-0.19 0 -0.1" />
        </visual>

<!--################################### drawer base ###########################################-->
        <visual>
                <geometry>              <!--1층 서랍-->
                        <box size="${body_link_x_dim-0.1} ${body_link_y_dim/1.2} ${body_link_z_dim}" />
                </geometry>
                <material name = "metallic_black_bar_W"/>
                <origin rpy="0 0 0" xyz="0 0 0.1" />
        </visual>

        <visual>                
                <geometry>              <!--1층 서랍 손잡이-->
                        <box size="${(body_link_x_dim-0.2)/2} ${body_link_y_dim/2} ${body_link_z_dim/3}" />
                </geometry>
                <material name = "metallic_black_bar"/>
                <origin rpy="0 0 0" xyz="0.13 0 0.1" />
        </visual>


        <visual>
                <geometry>              <!--1~2층 분리대 -->
                        <box size="${body_link_x_dim-0.099} ${body_link_y_dim/1.199} 0.01" />
                </geometry>
                <material name = "metallic_black"/>
                <origin rpy="0 0 0" xyz="0 0 0.2" />
        </visual>

        <visual>
                <geometry>              <!--2층 서랍-->
                        <box size="${body_link_x_dim-0.1} ${body_link_y_dim/1.2} ${body_link_z_dim}" />
                </geometry>
                <material name = "metallic_black_bar_W"/>
                <origin rpy="0 0 0" xyz="0 0 0.3" />
        </visual>

        <visual>                
                <geometry>              <!--2층 서랍 손잡이-->
                        <box size="${(body_link_x_dim-0.2)/2} ${body_link_y_dim/2} ${body_link_z_dim/3}" />
                </geometry>
                <material name = "metallic_black_bar"/>
                <origin rpy="0 0 0" xyz="0.13 0 0.28" />
        </visual>



<!--###############################collision part #######################################-->
        <collision>
                <geometry>
                        <box size="${body_link_x_dim} ${body_link_y_dim} ${body_link_z_dim}" />
                </geometry>
                <origin rpy="0 0 0" xyz="0 0 0" />
        </collision>
        

        <inertial>
                <origin rpy="0 0 0" xyz="0 0 0" />
                <mass value="${body_mass}"/>
        <inertia
        ixx="${(1/12)*body_mass*(body_link_y_dim*body_link_y_dim+body_link_z_dim*body_link_z_dim)}"
        ixy="0"
        ixz="0"
        iyy="${(1/12)*body_mass*(body_link_x_dim*body_link_x_dim+body_link_z_dim*body_link_z_dim)}"
        iyz="0"
        izz="${(1/12)*body_mass*(body_link_y_dim*body_link_y_dim+body_link_x_dim*body_link_x_dim)}" />
        </inertial>
</link>


<joint name="wheel1_joint" type="continuous">
        <parent link="base_link"/>
        <child link="wheel1_link"/>
        <origin xyz="${-body_link_x_dim/2+3*wheel_link_radius} ${-body_link_y_dim/1.55+wheel_link_length/2} ${wheel_link_z_location}"
        rpy = "0 0 0"/>
        <axis xyz="0 1 0"/>
        <limit effort="1000" velocity="1000"/>
        <dynamics damping="0.2" friction="0.0" />
</joint>



<link name="wheel1_link">
        <visual>
                <origin rpy="1.570795 0 0" xyz="0 0 0"/>
                <geometry>
                        <cylinder length="${wheel_link_length}" radius="${wheel_link_radius}"/>
                </geometry>
                 <material name = "changS"/>
              
        </visual>

        <collision>
                <origin rpy="1.570795 0 0" xyz="0 0 0"/>
                <geometry>
                        <cylinder length="${wheel_link_length}" radius="${wheel_link_radius}"/>
                </geometry>
        </collision>

        <xacro:inertia_wheel/> 
</link>

<!-- ##########################################################-->
<!-- END: BACK RIGTH WHEEL OF THE ROBOT AND THE JOINT-->
<!-- ##########################################################-->



<!-- ##########################################################-->
<!-- START : BACK LEFT WHEEL OF THE ROBOT AND THE JOINT-->
<!-- ##########################################################-->

<joint name="wheel2_joint" type="continuous">
        <parent link="base_link"/>
        <child link="wheel2_link"/>
        <origin xyz="${-body_link_x_dim/2+3*wheel_link_radius} ${body_link_y_dim/2+wheel_link_length/2} ${wheel_link_z_location}"
        rpy = "0 0 0"/>
        <axis xyz="0 1 0"/>
        <limit effort="1000" velocity="1000"/>
        <dynamics damping="0.2" friction="0.0" />
</joint>

<link name="wheel2_link">
        <visual>
                <origin rpy="1.570795 0 0" xyz="0 0 0" />
                <geometry>
                        <cylinder length="${wheel_link_length}" radius="${wheel_link_radius}"/>
                </geometry>
                <material name = "changS"/>
        </visual>

        <collision>
                <origin rpy="1.570795 0 0" xyz="0 0 0"/>
                <geometry>
                        <cylinder length="${wheel_link_length}" radius="${wheel_link_radius}"/>
                </geometry>
        </collision>
        <xacro:inertia_wheel/>
</link>

<!-- ##########################################################-->
<!-- END: BACK LEFT WHEEL OF THE ROBOT AND THE JOINT-->
<!-- ##########################################################-->

<!-- ##########################################################-->
<!-- START : FRONT RIGTH WHEEL OF THE ROBOT AND THE JOINT-->



<!-- ##########################################################-->
<!-- START : caster right & back-->

 <joint name = "caster_wheel_joint_1" type = "fixed">
        <parent link = "base_link"/>
        <child link = "caster_wheel_1"/>
        <origin xyz="${-body_link_x_dim/2+0.09*wheel_link_radius} ${-body_link_y_dim/1.46+wheel_link_length/2} ${wheel_link_z_location-0.04}"
        rpy = "0 0 0"/>
        <axis xyz="0 0 1"/>
        <!--limit lower="-3.14159" upper="3.14159" effort="1000" velocity="1000"/-->
  </joint>

<link name="caster_wheel_1">
        <visual>
        <origin xyz="0 0 0" rpy = "1.570795 0 0"/>
        <geometry>
                <cylinder length="${wheel_link_length}" radius="${caster_link_radius}"/>
        </geometry>
        <material name = "changS"/>
        </visual>

        <collision>
        <origin xyz="0 0 0" rpy = "1.570795 0 0"/>
        <geometry>
                <cylinder length="${wheel_link_length}" radius="${caster_link_radius}"/>
        </geometry>
    </collision>
    <!--xacro:inertia_wheel/--> 
  </link>



<!-- ##########################################################-->
<!-- START : caster left & back-->

<joint name = "caster_wheel_joint_2" type = "fixed">
        <parent link = "base_link"/>
        <child link = "caster_wheel_2"/>
        <origin xyz="${-body_link_x_dim/2+0.09*wheel_link_radius} ${+body_link_y_dim/1.46-wheel_link_length/2} ${wheel_link_z_location-0.04}"
        rpy = "0 0 0"/>
        <axis xyz="0 0 1"/>
        <!--limit lower="-3.14159" upper="3.14159" effort="1000" velocity="1000"/-->
        
  </joint>

<link name="caster_wheel_2">
        <visual>
        <origin xyz="0 0 0" rpy = "1.570795 0 0"/>
        <geometry>
                <cylinder length="${wheel_link_length}" radius="${caster_link_radius}"/>
        </geometry>
        <material name = "changS"/>
        </visual>

        <collision>
        <origin xyz="0 0 0" rpy = "1.570795 0 0"/>
        <geometry>
                <cylinder length="${wheel_link_length}" radius="${caster_link_radius}"/>
        </geometry>
    </collision>
    <!--xacro:inertia_wheel/--> 
  </link>



<!-- #####################################################################-->
<!--LIDAR PART-->

  <joint name = "head_scanner" type = "fixed">
    <origin xyz = "0 0 0.45" rpy = "0 0 0"/>
    <parent link = "base_link"/>
    <child link = "base_scan"/>
  </joint>


<link name="base_scan">



        <visual>
                <geometry>              <!--지지판 -->
                        <box size="0.2 0.2 0.05" />
                </geometry>
                <material name = "metallic_black"/>
                <origin rpy="0 0 0" xyz="0 0 -0.026" />
        </visual>


        <visual>                        <!-- lidar -->
        <origin xyz="0 0 0.0395" rpy = "0 0 1.570795"/>
                <geometry>
                        <cylinder length="0.08" radius="0.06"/>
                </geometry>
        <material name="coral"/>
        </visual>






<!-- ################## Lidar collision part ###########################-->
        <collision>
        <origin xyz="0 0 0.457" rpy = "0 0 1.570795"/>
                <geometry>
                        <cylinder length="0.1" radius="0.09"/>
                </geometry>
        </collision>
</link>
<!-- ##########################################################-->
<!-- END: FRONT RIGHT WHEEL OF THE ROBOT AND THE JOINT-->
<!-- ##########################################################-->
</robot>



