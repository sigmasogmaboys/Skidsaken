print("skidsaken loading...")

-- ============================================================
-- ERROR HANDLER
-- ============================================================
local function flashError(msg)
    local ok, flashErr = pcall(function()
        local player = game:GetService("Players").LocalPlayer
        local gui = player:FindFirstChild("PlayerGui") or player:WaitForChild("PlayerGui", 5)
        if not gui then return end

        local errorGui = Instance.new("ScreenGui")
        errorGui.Name = "Skidsaken_Error"
        errorGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
        errorGui.Parent = gui

        local frame = Instance.new("Frame")
        frame.Size = UDim2.new(0, 500, 0, 100)
        frame.Position = UDim2.new(0.5, -250, 0.5, -50)
        frame.BackgroundColor3 = Color3.fromRGB(30, 0, 0)
        frame.BackgroundTransparency = 0.15
        frame.BorderSizePixel = 3
        frame.BorderColor3 = Color3.fromRGB(255, 50, 50)
        frame.Parent = errorGui

        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 8)
        corner.Parent = frame

        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(1, -20, 1, -20)
        label.Position = UDim2.new(0, 10, 0, 10)
        label.Text = "⚠️ Skidsaken ERROR\n\n" .. tostring(msg):sub(1, 150)
        label.TextColor3 = Color3.fromRGB(255, 200, 200)
        label.TextScaled = true
        label.BackgroundTransparency = 1
        label.TextWrapped = true
        label.TextXAlignment = Enum.TextXAlignment.Left
        label.Font = Enum.Font.GothamBold
        label.Parent = frame

        local closeBtn = Instance.new("TextButton")
        closeBtn.Size = UDim2.new(0, 80, 0, 30)
        closeBtn.Position = UDim2.new(1, -90, 1, -40)
        closeBtn.Text = "Dismiss"
        closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
        closeBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        closeBtn.BorderSizePixel = 0
        closeBtn.Font = Enum.Font.GothamBold
        closeBtn.Parent = frame

        local closeCorner = Instance.new("UICorner")
        closeCorner.CornerRadius = UDim.new(0, 4)
        closeCorner.Parent = closeBtn

        closeBtn.MouseButton1Click:Connect(function()
            errorGui:Destroy()
        end)

        task.delay(15, function()
            pcall(function() errorGui:Destroy() end)
        end)
    end)
    if not ok then warn("[skidsaken] flashError itself failed: " .. tostring(flashErr)) end
end

local success, err = pcall(function()

local svc = {
    Players        = game:GetService("Players"),
    Run            = game:GetService("RunService"),
    Input          = game:GetService("UserInputService"),
    RS             = game:GetService("ReplicatedStorage"),
    WS             = game:GetService("Workspace"),
    TweenService   = game:GetService("TweenService"),
    TextChat       = game:GetService("TextChatService"),
    Http           = game:GetService("HttpService"),
    SoundService   = game:GetService("SoundService"),
    Stats          = game:GetService("Stats"),
    Debris         = game:GetService("Debris"),
}

local lp  = svc.Players.LocalPlayer
local gui = lp:WaitForChild("PlayerGui", 10)

-- ============================================================
-- DELTA / GENV COMPATIBILITY SHIMS
-- Supports Delta, Genv, Fluxus, Synapse X, KRNL, and others
-- ============================================================
local _genv = (getgenv and getgenv()) or _G or {}

local function _safeGet(name)
    return rawget(_genv, name) or (type(getfenv) == "function" and getfenv(0)[name]) or nil
end

local _isfolder    = _safeGet("isfolder")    or function() return false end
local _makefolder  = _safeGet("makefolder")  or function() end
local _writefile   = _safeGet("writefile")   or function() end
local _isfile      = _safeGet("isfile")      or function() return false end
local _readfile    = _safeGet("readfile")    or function() return "" end
local _getcustom   = _safeGet("getcustomasset") or function(p) return p end
local _setclip     = _safeGet("setclipboard") or _safeGet("toclipboard") or function() end

-- Patch global setclipboard to use our resolved version (Delta uses toclipboard)
if not rawget(_genv, "setclipboard") and rawget(_genv, "toclipboard") then
    _genv.setclipboard = _genv.toclipboard
end

-- Delta uses syn.request or http_request; normalise to game:HttpGet style fallback
local _httpGet = function(url)
    -- Try game:HttpGet first (works on most executors including Delta)
    local ok, res = pcall(function() return game:HttpGet(url, true) end)
    if ok and res and #res > 10 then return res end
    -- Fallback: syn.request / http_request / request
    local reqFn = _safeGet("syn") and _safeGet("syn").request
        or _safeGet("http_request")
        or _safeGet("request")
    if reqFn then
        local ok2, resp = pcall(reqFn, { Url = url, Method = "GET" })
        if ok2 and resp and resp.Body then return resp.Body end
