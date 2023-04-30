Download Link: https://assignmentchef.com/product/solved-graphics-lab7-skybox
<br>
<u>Lab 7</u>

Setting up a skybox…

Change the Texture.h as follows

Add the following methods…

GLuint loadCubemap(std::vector&lt;std::string&gt; faces);

GLint getTexHandler() { return textureHandler; }




Addthe following method to the texture.cpp

GLuint Texture::loadCubemap(std::vector&lt;std::string&gt; faces)

{

glGenTextures(1, &amp;textureHandler);

glBindTexture(GL_TEXTURE_CUBE_MAP, textureHandler);




int width, height, nrChannels;

for (unsigned int i = 0; i &lt; faces.size(); i++)

{

unsigned char *data = stbi_load(faces[i].c_str(), &amp;width, &amp;height, &amp;nrChannels, 0);

if (data)

{

glTexImage2D(GL_TEXTURE_CUBE_MAP_POSITIVE_X + i,

0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data

);

stbi_image_free(data);

}

else

{

std::cout &lt;&lt; “Cubemap texture failed to load at path: ” &lt;&lt; faces[i] &lt;&lt; std::endl;

stbi_image_free(data);

}

}

glTexParameteri(GL_TEXTURE_CUBE_MAP, GL_TEXTURE_MIN_FILTER, GL_LINEAR);

glTexParameteri(GL_TEXTURE_CUBE_MAP, GL_TEXTURE_MAG_FILTER, GL_LINEAR);

glTexParameteri(GL_TEXTURE_CUBE_MAP, GL_TEXTURE_WRAP_S, GL_CLAMP_TO_EDGE);

glTexParameteri(GL_TEXTURE_CUBE_MAP, GL_TEXTURE_WRAP_T, GL_CLAMP_TO_EDGE);

glTexParameteri(GL_TEXTURE_CUBE_MAP, GL_TEXTURE_WRAP_R, GL_CLAMP_TO_EDGE);




return textureHandler;

}




In the maingame.h add the following…




void Skybox();




GLuint skyboxVAO, skyboxVBO, cubemapTexture;

vector&lt;std::string&gt; faces;

Texture skybox;

Shader shaderSkybox;




In the maingame add the following to the init method…

vector&lt;std::string&gt; faces

{

“..\res\skybox\right.jpg”,

“..\res\skybox\left.jpg”,

“..\res\skybox\top.jpg”,

“..\res\skybox\bottom.jpg”,

“..\res\skybox\front.jpg”,

“..\res\skybox\back.jpg”

};




cubemapTexture = skybox.loadCubemap(faces); //Load the cubemap using “faces” into cubemapTextures




float skyboxVertices[] = {

// positions

-6.0f,  6.0f, -6.0f,

-6.0f, -6.0f, -6.0f,

6.0f, -6.0f, -6.0f,

6.0f, -6.0f, -6.0f,

6.0f,  6.0f, -6.0f,

-6.0f,  6.0f, -6.0f,




-6.0f, -6.0f,  6.0f,

-6.0f, -6.0f, -6.0f,

-6.0f,  6.0f, -6.0f,

-6.0f,  6.0f, -6.0f,

-6.0f,  6.0f,  6.0f,

-6.0f, -6.0f,  6.0f,




6.0f, -6.0f, -6.0f,

6.0f, -6.0f,  6.0f,

6.0f,  6.0f,  6.0f,

6.0f,  6.0f,  6.0f,

6.0f,  6.0f, -6.0f,

6.0f, -6.0f, -6.0f,




-6.0f, -6.0f,  6.0f,

-6.0f,  6.0f,  6.0f,

6.0f,  6.0f,  6.0f,

6.0f,  6.0f,  6.0f,

6.0f, -6.0f,  6.0f,

-6.0f, -6.0f,  6.0f,




-6.0f,  6.0f, -6.0f,

6.0f,  6.0f, -6.0f,

6.0f,  6.0f,  6.0f,

6.0f,  6.0f,  6.0f,

-6.0f,  6.0f,  6.0f,

-6.0f,  6.0f, -6.0f,




-6.0f, -6.0f, -6.0f,

-6.0f, -6.0f,  6.0f,

6.0f, -6.0f, -6.0f,

6.0f, -6.0f, -6.0f,

-6.0f, -6.0f,  6.0f,

6.0f, -6.0f,  6.0f

};




//use openGL functionality to generate &amp; bind data into buffers

glGenVertexArrays(1, &amp;skyboxVAO);

glGenBuffers(1, &amp;skyboxVBO);

glBindVertexArray(skyboxVAO);

glBindBuffer(GL_ARRAY_BUFFER, skyboxVBO);

glBufferData(GL_ARRAY_BUFFER, sizeof(skyboxVertices), &amp;skyboxVertices, GL_STATIC_DRAW);

glEnableVertexAttribArray(0);

glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);




}




Add the following method…

void MainGame::Skybox()

{

glDepthFunc(GL_LEQUAL);  // change depth function so depth test passes when values are equal to depth buffer’s content

shaderSkybox.Bind();

shaderSkybox.setInt(“skybox”, 0);

//view = glm::mat4(glm::mat3(camera.GetViewMatrix())); // remove translation from the view matrix

shaderSkybox.setMat4(“view”, myCamera.GetView());

shaderSkybox.setMat4(“projection”, myCamera.GetProjection());

// skybox cube

glBindVertexArray(skyboxVAO);

glActiveTexture(GL_TEXTURE0);

glBindTexture(GL_TEXTURE_CUBE_MAP, cubemapTexture);

glDrawArrays(GL_TRIANGLES, 0, 36);

glBindVertexArray(0);

glDepthFunc(GL_LESS); // set depth function back to default

}




TODO..

Initialise the skybox shader

Draw the skybox

Run the code, find and fix the error!


