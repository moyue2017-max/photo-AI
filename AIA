import streamlit as st
import requests
import time

# 页面配置
st.set_page_config(
    page_title="AI 图片生成器",
    page_icon="🎨",
    layout="wide"
)

# 标题
st.title("🎨 AI 图片生成器")
st.markdown("输入文字描述，AI 帮你生成图片！")
st.markdown("---")

# 侧边栏
with st.sidebar:
    st.header("⚙️ 设置")
    
    # API 选择
    api_choice = st.radio(
        "选择 AI 模型",
        ["Stable Diffusion (免费)", "示例模式（不调用API）"]
    )
    
    st.markdown("---")
    st.caption("💡 提示：描述越详细，生成效果越好")
    st.caption("🌟 推荐格式：主体 + 风格 + 细节")

# 主区域
col1, col2 = st.columns([3, 2])

with col1:
    st.subheader("📝 输入你的创意")
    
    # 提示词输入
    prompt = st.text_area(
        "描述你想生成的图片",
        placeholder="例如：一只穿着宇航服的猫，在月球上跳舞，赛博朋克风格，高清，细节丰富",
        height=100
    )
    
    # 示例提示词
    with st.expander("💡 查看示例提示词"):
        st.code("一只可爱的柴犬，戴着墨镜，在海滩上冲浪")
        st.code("未来科技城市，霓虹灯，赛博朋克，夜景，4K高清")
        st.code("梵高风格的星空下，一座中国古典园林")
        st.code("机器人和人类一起喝咖啡，温馨，卡通风格")
    
    # 生成按钮
    generate_btn = st.button("🎨 生成图片", use_container_width=True, type="primary")

with col2:
    st.subheader("🖼️ 生成结果")
    
    # 显示区域
    image_placeholder = st.empty()

# 生成逻辑
if generate_btn:
    if not prompt:
        st.warning("⚠️ 请先输入提示词！")
    else:
        with st.spinner("🎨 AI 正在创作中，请稍候..."):
            
            if api_choice == "示例模式（不调用API）":
                # 示例模式：使用固定图片演示
                time.sleep(2)  # 模拟生成时间
                
                # 使用一个公开的示例图片URL
                demo_url = "https://picsum.photos/512/512"
                
                with col2:
                    st.success("✅ 生成成功！")
                    st.image(demo_url, caption=f"提示词：{prompt}", use_container_width=True)
                    st.download_button(
                        label="💾 下载图片",
                        data=requests.get(demo_url).content,
                        file_name="ai_generated.jpg",
                        mime="image/jpeg"
                    )
                
            else:
                # 真实 API 调用（Hugging Face）
                try:
                    API_URL = "https://api-inference.huggingface.co/models/stabilityai/stable-diffusion-2-1"
                    
                    # 注意：这需要 Hugging Face API token
                    # 免费注册：https://huggingface.co/settings/tokens
                    headers = {
                        "Authorization": "Bearer 你的API密钥"  # 需要替换
                    }
                    
                    response = requests.post(
                        API_URL,
                        headers=headers,
                        json={"inputs": prompt}
                    )
                    
                    if response.status_code == 200:
                        with col2:
                            st.success("✅ 生成成功！")
                            st.image(response.content, caption=f"提示词：{prompt}", use_container_width=True)
                            st.download_button(
                                label="💾 下载图片",
                                data=response.content,
                                file_name="ai_generated.png",
                                mime="image/png"
                            )
                    else:
                        st.error("❌ 生成失败，请稍后重试")
                        st.error(f"错误信息：{response.text}")
                
                except Exception as e:
                    st.error(f"❌ 出错了：{e}")

# 底部说明
st.markdown("---")
st.markdown("""
### 📖 使用说明
1. **输入提示词**：描述你想生成的图片
2. **点击生成**：等待 AI 创作（约 10-30 秒）
3. **下载保存**：满意的话可以下载图片

### 💡 提示词技巧
- **主体明确**：说清楚要画什么（猫、房子、人物等）
- **添加风格**：指定艺术风格（写实、卡通、油画等）
- **细节描述**：光线、颜色、氛围等
- **质量词**：high quality, 4K, detailed 等

### ⚠️ 注意事项
- 示例模式返回随机图片，仅供演示
- 真实 API 需要注册并获取密钥
- 生成速度取决于服务器负载
""")
