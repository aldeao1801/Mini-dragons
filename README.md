```
-- Configurações iniciais
local scriptAtivo = true
local fovRaio = 300
local corJogadorComArma = {1, 0, 0} -- Vermelho
local corJogadorSemArma = {0, 0, 1} -- Azul
local aimbotAtivo = false

-- Função para desenhar caixas ao redor dos jogadores
local function desenharCaixa(jogador, cor)
    -- Desenhar uma caixa ao redor do jogador
    local x, y = jogador.posicao.x, jogador.posicao.y
    local largura, altura = jogador.tamanho.largura, jogador.tamanho.altura
    
    -- Verificar se o jogador está dentro do FOV
    local distancia = calcularDistancia(jogador.posicao, meuJogador.posicao)
    if distancia <= fovRaio then
        love.graphics.setColor(cor)
        love.graphics.setLineWidth(2)
        love.graphics.rectangle("line", x, y, largura, altura)
    end
end

-- Função para desenhar FOV e setas apontando para os jogadores
local function desenharFOVeSetas(meuJogador, jogador)
    local distancia = calcularDistancia(meuJogador.posicao, jogador.posicao)
    if distancia <= fovRaio then
        -- Desenhar o círculo do FOV
        love.graphics.setColor(0, 1, 0) -- Verde
        love.graphics.circle("line", meuJogador.posicao.x, meuJogador.posicao.y, fovRaio)
        
        -- Desenhar uma seta apontando para o jogador
        love.graphics.setColor(1, 1, 0) -- Amarelo
        love.graphics.setLineWidth(2)
        love.graphics.line(meuJogador.posicao.x, meuJogador.posicao.y, jogador.posicao.x, jogador.posicao.y)
    end
end

-- Função para atualizar o script
local function update()
    if scriptAtivo then
        local jogadores = obterTodosJogadores()
        local meuJogador = obterMeuJogador()
        for _, jogador in pairs(jogadores) do
            local cor = jogador.possuiArma and corJogadorComArma or corJogadorSemArma
            desenharCaixa(jogador, cor)
            desenharFOVeSetas(meuJogador, jogador)
            
            -- Aimbot
            if aimbotAtivo and jogador.possuiArma then
                local distancia = calcularDistancia(meuJogador.posicao, jogador.posicao)
                if distancia <= fovRaio then
                    -- Ajustar a mira para o jogador
                    meuJogador.mira.x = jogador.posicao.x
                    meuJogador.mira.y = jogador.posicao.y
                end
            end
        end
    end
end

-- Função para desativar/ativar o script
local function alternarScript()
    scriptAtivo = not scriptAtivo
end

-- Função para ativar/desativar o aimbot
local function alternarAimbot()
    aimbotAtivo = not aimbotAtivo
end

-- Loop principal do script
function love.update(dt)
    update()
end

function love.draw()
    -- Não é necessário fazer nada aqui
end

function love.keypressed(key)
    if key == "f1" then
        alternarScript()
    elseif key == "f2" then
        alternarAimbot()
    end
end
```
