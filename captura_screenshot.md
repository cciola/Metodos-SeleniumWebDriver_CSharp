# Captura de screenshot

Após criar um Unit Test Project e importar as bibliotecas abaixo:
```
NUnit
NUnit 3 - NUnit Project Loader Extension
NUnit 3 - NUnit V2 Framework Driver Extension
NUnit 3 - NUnit V2 Result Writer Extension
NUnit 3 - Team City Event Listener Extension
NUnit 3 - Visual Studio Project Loader Extension
NUnit Console Runner Version 3 (No Extensions)
NUnit Console Runner Version 3 With Extensions
NUnit Console Version 3
NUnit Test Adapter for VS2012, VS2013 ans VS2015
Selenium WebDriver
Selenium WebDriver Support Classes
Selenium.WebDriver.ChromeDriver //caso deseje executar testes no navegador Internet Explorer
Selenium.WebDriver.IEDriver //caso deseje executar testes no navegador Internet Explorer
Selenium.WebDriver.Firefox //caso deseje executar testes no navegador Internet Explorer
Selenium.Support
```
Na classe pública do projeto, localizada logo abaixo do [TextFixture], informe as seguintes variáveis:
```csharp
    [TestFixture]
    public class NomeDoProjeto
    {
        public IWebDriver driver;
        private string baseURL;
        public string screenshotsPasta;
        int contador = 1;
```
Temos:
- <i>public IWebDriver driver;</i> declara o IWebDriver.
- <i>private string baseURL;</i> variável que receberá a URL base do site a ser acessado.
- <i>public string screenshotsPasta;</i> variável que receberá o caminho da pasta em que os screenshots serão salvos.
- <i>int contador = 1;</i> variável referente ao contador da numeração dos arquivos.
<br></br>
Logo após esta classe, crie a classe abaixo:
```csharp
        public void Screenshot(IWebDriver driver, string localArquivo)
        {
            ITakesScreenshot camera = driver as ITakesScreenshot;
            Screenshot foto = camera.GetScreenshot();
            foto.SaveAsFile(localArquivo, ImageFormat.Png);
        }
```
Nela, temos:
- <i>ITakesScreenshot camera = driver as ITakesScreenshot;</i> é a instrução que indica captura de screenshot atribuída à variável <i>camera</i>.
- <i>Screenshot foto = camera.GetScreenshot();</i> indica a captura do screenshot na variável <i>foto</i> pela <i>camera</i>.
- <i>foto.SaveAsFile(localArquivo, ImageFormat.Png);</i> indica o caminho onde o arquivo será salvo, e a extensão (no caso, .png).
<br>
O próximo passo é indicar o caminho da pasta dentro da classe <i>SetupTest</i>, constante em [SetUp], e depois criar outra classe para gerar o nome do arquivo mais a extensão, e salvá-lo na pasta indicada. Isto é declarado dentro de [Setup]:
```csharp
    public void SetupTest()
    {
        driver = new ChromeDriver();
        driver.Manage().Window.Maximize();
        baseURL = "https://www.google.com.br";
        screenshotsPasta = @"C:\Users\cciola\Desktop\Selenium\Evidencias\";
    }
```    
Temos:
- <i>driver = new ChromeDriver();</i> indica o driver do navegador a ser testado (no caso, Chrome);
- <i>driver.Manage().Window.Maximize();</i> maximiza a janela do navegador, no momento da execução do teste.
- <i>baseURL = "https://www.google.com.br";</i> URL base a ser acessada e incrementada posteriormente com o endereço que se deseja acessar.
- <i>screenshotsPasta</i> apresenta o caminho onde o arquivo será salvo (pasta existente).
```csharp        
    public void capturaImagem()
    {
        Screenshot(driver, screenshotsPasta + "Imagem_" + contador++ + ".png");
        Thread.Sleep(500);
    }
```
Temos:
- <i>Screenshot</i> concatenando o nome do arquivo da imagem mais o incremento da variável <i>contador</i>, que servirá para numerar sequencialmente os arquivos a serem salvos.
- <i>Thread.Sleep(500)</i> que conta o tempo em meio segundo para que a captura seja efetuada pelo driver.

# Veja o método funcionando

Copie e cole o código abaixo, dê um Build e execute o teste.
```csharp
using System;
using System.Text;
using System.Text.RegularExpressions;
using System.Threading;
using System.Drawing.Imaging;
using NUnit.Framework;
using OpenQA.Selenium;
using OpenQA.Selenium.Firefox;
using OpenQA.Selenium.Support.UI;
using OpenQA.Selenium.Chrome;
using System.IO;
using System.Collections;

namespace SeleniumTests
{
    [TestFixture]
    public class NomeDoProjeto
    {
        public IWebDriver driver;
        private string baseURL;
        public string screenshotsPasta;
        int contador = 1;


        //Método para capturar screenshot da tela
        public void Screenshot(IWebDriver driver, string localArquivo)
        {
            ITakesScreenshot camera = driver as ITakesScreenshot;
            Screenshot foto = camera.GetScreenshot();
            foto.SaveAsFile(localArquivo, ImageFormat.Png);
        }

        [SetUp]
        public void SetupTest()
        {
            driver = new ChromeDriver();
            driver.Manage().Window.Maximize();
            baseURL = "https://www.google.com.br";
            screenshotsPasta = @"C:\Users\cciola\Documents\Visual Studio 2013\Projects\TesteGit\Evidencias\";
        }

        public void capturaImagem()
        {
           Screenshot(driver, screenshotsPasta + "Imagem_" + contador++ + ".png");
           Thread.Sleep(500);
        }

        [TearDown]
        public void TeardownTest()
        {
            try
            {
                driver.Quit();
            }
            catch (Exception)
            {
                // Ignore errors if unable to close the browser
            }
        }

        [Test]

        public void NomeDoTeste()
        {
            driver.Navigate().GoToUrl(baseURL + "/intl/pt-BR/about/");
            Thread.Sleep(1000);
            capturaImagem();
            Thread.Sleep(1000);         
        }
    }
}
```
O código acessa a página base <i>www.google.com.br</i> concatenada ao endereço da URL <i>/intl/pt-BR/about/</i> e captura um screenshot da página. Ao acessar a pasta informada, o resultado será um arquivo com o screenshot salvo com o nome desejado, mais a numeração.
<br></br>
Dúvidas me contate! carol.ciola@gmail.com
