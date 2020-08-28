## Buffers and Buffer Views  
**1. BufferD3D11Impl**  
* 实现ID3D11Buffer接口  
* 可以绑定到流水线uniform、shader resource(SRV)、unordered access view(UAV)、顶点缓冲、索引缓冲  
* D3D11方法：ID3D11Device::CreateBuffer()  

**2. BufferViewD3D11Impl**  
* 实现ID3D11View接口  
* 通过调用BufferD3D11Impl::CreateViewInternal()来创建，然后会根据类型反过来再调用BufferD3D11Impl::CreateSRV()或者BufferD3D11Impl::CreateUAV()  

## Textures and Texture Views  
**1. TextureBaseD3D11Impl，实现ITexture接口的基类**  
* 调用RenderDeviceD3D11Impl::CreateTexture()创建  
* Textures可以绑定到shader resource(SRV)、shader resource(SRV)、render target(RTV)、depth-stencil(DSV)、unordered access view(UAV)  
* TextureBaseD3D11::CreateViewInternal()方法会调用CreateSRV()、CreateRTV()、CreateDSV()、CreateUAV()，子类定义具体实现  
* D3D11方法：D3D11Device::CreateTexture1D()、D3D11Device::CreateTexture2D()、D3D11Device::CreateTexture3D()


**2. Texture1D_D3D11Impl，实现1D texture**  
**3. Texture2D_D3D11Impl，实现2D texture**  
**4. Texture3D_D3D11Impl，实现3D texture**  
**5. TextureViewD3D11Impl，实现ITextureView接口**  
* CreateSRV()、CreateRTV()、CreateDSV()、CreateUAV()创建  

## Samplers  
**1. SamplerD3D11Impl，实现ISampler接口**  
* D3D11方法：ID3D11Device::CreateSamplerState()  

## Shaders  
**1. ShaderD3D11Impl，实现IShader接口**  
* Pipeline State的一部分  
* 加载shader文件、编译、初始化ShaderResourcesD3D11对象  
* 初始化static resource layout (references static shader resources)
* 初始化static shader resource cache (keeps references to objects bound as static resources)  

## Shader Resource Management System  
主要包含两个组件：ShaderResourceLayoutD3D11和ShaderResourceCacheD3D11。ShaderResourceLayoutD3D11决定了资源如何map到shader。ShaderResourceCacheD3D11引用了已经绑定到shader的资源的对象。用到的地方有两个：  

**1. shader对象，ShaderD3D11Impl，包含一个shader resource layout来管理static shader resource。**  
* The layout object is bound to the shader resource cache that provides storage for object references  

**2. ShaderResourceBindingD3D11Impl，包含shader resource cache和shader resource layout**  
* Every resource layout object is connected to corresponding shader resource cache that provides storage for object references  

## Pipeline State  
* Blend state (ID3D11BlendState)  
* Rasterizer state (ID3D11RasterizerState)  
* Depth-stencil state (ID3D11DepthStencilState)  
* Input layout (ID3D11InputLayout)  
* Shader stages  

## Render Device  

主要负责创建引擎对象：  
* buffers: RenderDeviceD3D11Impl::CreateBuffer()  
* textures: RenderDeviceD3D11Impl::CreateTexture()  
* samplers: RenderDeviceD3D11Impl::CreateSampler()  
* shaders: RenderDeviceD3D11Impl::CreateShader()  
* pipeline states: RenderDeviceD3D11Impl::CreatePipelineState()  

## Device Context  
**1. DeviceContextD3D11Impl，实现IDeviceContext接口**  
* 配置流水线的各个阶段：pipeline state, vertex and index buffers, blend factors，etc  
* 实现draw指令，分发compute指令  
* 跟踪当前绑定状态，已避免冗余状态切换  
* 转换资源到正确状态，提交到gpu流水线  

**2. CommandListD3D11Impl，实现ICommandList接口**  




