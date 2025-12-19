local Translations = {
    ["Auto Ddakji (Remote)"] = "自动打画片(遥控)"
    ["Auto飞行ing Stone (Remote)"]="自动打石子(遥控)"
    ["Auto Gonggi (Remote)"]="自动抓石子(遥控)"
    ["Auto Spinning Top (Remote)]"="自动陀螺(遥控)"
    ["Auto Jegi (Remote)"]="自动踢毽子(遥控)"
    ["Gonggi Auto QTE"]="自动抓石子"
    ["Spinning Top Auto Balance"]="自动陀螺"
}

local function translateText(text)
    if not text or type(text) ~= "string" then return text end
    if Translations[text] then return Translations[text] end
    local translated = text
    for en, cn in pairs(Translations) do
        translated = translated:gsub(en, cn)
    end
    return translated
end

local translatedElements = {}
local originalTexts = {}

local function safeTranslateElement(element)
    if translatedElements[element] then return end
    
    pcall(function()
        if element:IsA("TextLabel") or element:IsA("TextButton") or element:IsA("TextBox") then
            local currentText = element.Text
            if currentText and currentText ~= "" then
                if not originalTexts[element] then
                    originalTexts[element] = currentText
                end
                
                local translatedText = translateText(currentText)
                if translatedText ~= currentText then
                    element.Text = translatedText
                    translatedElements[element] = true
                end
            end
        end
    end)
end

local function reTranslateAllUI()
    translatedElements = {}
    for _, gui in ipairs(game:GetService("CoreGui"):GetDescendants()) do
        safeTranslateElement(gui)
    end
    local player = game:GetService("Players").LocalPlayer
    if player and player:FindFirstChild("PlayerGui") then
        for _, gui in ipairs(player.PlayerGui:GetDescendants()) do
            safeTranslateElement(gui)
        end
    end
end

local function setupSmartListener()
    local function onTextPropertyChanged(element)
        if translatedElements[element] then
            local currentText = element.Text
            local originalText = originalTexts[element]
            
            if currentText == originalText or translateText(currentText) == currentText then
                translatedElements[element] = nil
                return
            end
            
            local translatedText = translateText(currentText)
            if translatedText ~= currentText then
                element.Text = translatedText
            end
        else
            safeTranslateElement(element)
        end
    end

    local function addSmartListener(parent)
        for _, desc in ipairs(parent:GetDescendants()) do
            safeTranslateElement(desc)
        end
        parent.DescendantAdded:Connect(function(descendant)
            task.wait(0.3)
            safeTranslateElement(descendant)
            
            if descendant:IsA("TextLabel") or descendant:IsA("TextButton") or descendant:IsA("TextBox") then
                descendant:GetPropertyChangedSignal("Text"):Connect(function()
                    task.wait(0.1)
                    onTextPropertyChanged(descendant)
                end)
            end
        end)
    end
    
    pcall(addSmartListener, game:GetService("CoreGui"))
    local player = game:GetService("Players").LocalPlayer
    if player and player:FindFirstChild("PlayerGui") then
        pcall(addSmartListener, player.PlayerGui)
    end
end

local function delayedInitialTranslate()
    task.wait(5)
    reTranslateAllUI()
end

task.wait(2)
setupSmartListener()
delayedInitialTranslate()

local success, err = pcall(function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/hdjsjjdgrhj/script-hub/refs/heads/main/AX%20CN"))()
    task.wait(1.5)
    reTranslateAllUI()
end)

if not success then
    warn("加载失败:", err)
end
