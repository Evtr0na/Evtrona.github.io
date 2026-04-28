在文本修改器中输入以下代码并运行：

```Python
import bpy
import os

# --- 请再次确认你的 Icons 文件夹真实路径 ---
icon_folder = r"在这里输入你的 Icons 文件夹真实路径"

def link_external_icons_fixed():
    count = 0
    missing = 0
    print("--- 正在使用 5.0 新语法修复图标 ---")

    for brush in bpy.data.brushes:
        # 必须先标记为资产才能加载预览图
        if not brush.asset_data:
            continue
            
        # 匹配 PNG 图标
        icon_name = brush.name + ".png"
        icon_path = os.path.join(icon_folder, icon_name)
        
        if os.path.exists(icon_path):
            try:
                # --- 关键修复：使用 5.0 的 temp_override 语法 ---
                with bpy.context.temp_override(id=brush):
                    bpy.ops.ed.lib_id_load_custom_preview(filepath=icon_path)
                
                count += 1
                if count % 50 == 0: # 每成功50个打印一次进度，防止卡死
                    print(f"进度：已完成 {count} 个...")
            except Exception as e:
                print(f"错误：笔刷 {brush.name} 加载失败: {e}")
        else:
            missing += 1

    print(f"--- 任务完成 ---")
    print(f"成功导入: {count} 个")
    print(f"未找到对应图片: {missing} 个")

# 执行函数
link_external_icons_fixed()
```