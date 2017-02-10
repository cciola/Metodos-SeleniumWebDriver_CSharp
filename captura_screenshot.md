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
Selenium.WebDriver.ChromeDriver
Selenium.WebDriver.IEDriver
Selenium.Support
Selenium.WebDriver.ChromeDriver
Selenium.WebDriver.IEDriver
```
Na classe pública do projeto, localizada logo abaixo do [TextFixture], informe as seguintes variáveis:
```
    [TestFixture]
    public class NomeDoProjeto
    {
        public IWebDriver driver;
        public string localArquivo;
        int contador = 0;
```
Crie a classe abaixo:
```
  public void Screenshot(IWebDriver driver, string localArquivo)
  {
      ITakesScreenshot camera = driver as ITakesScreenshot;
      Screenshot printscreen = camera.GetScreenshot();
      printscreen.SaveAsFile(localArquivo, ImageFormat.Png);
      screenshotsPasta = @"C:\Users\cciola\Desktop\Selenium\Evidencias\";
  }
```
Nela, temos:
- <i>ITakesScreenshot camera = driver as ITakesScreenshot;</i> é a instrução que indica captura de screenshot atribuída à variável <i>camera</i>.
- <i>Screenshot foto = camera.GetScreenshot();</i> indica a captura do screenshot na variável <i>foto</i> pela <i>camera</i>.
- <i>foto.SaveAsFile(localArquivo, ImageFormat.Png);</i> indica o local onde o arquivo será salvo, e a extensão (no caso, .png).
- <i>screenshotsPasta = @"C:\Users\cciola\Desktop\Selenium\Evidencias\";</i> indica o caminho da pasta na qual os arquivos de screenshot serão armazenados.
<br>
O próximo passo é gerar o nome do arquivo mais a extensão, e salvá-lo na pasta indicada. Isto é declarado dentro de [Setup]:
```
    public void capturaImagem() // Gera 
    {
        Screenshot(driver, screenshotsPasta + "Imagem_" + contador++ + ".png");
        Thread.Sleep(500);
    }
```
Temos:
- <i>Screenshot</i> concatenando o nome do arquivo da imagem mais o incremento da variável <i>contador</i>, que servirá para numerar sequencialmente os arquivos a serem salvos.
- <i>Thread.Sleep(500)</i> que conta o tempo em meio segundo para que a captura seja efetuada pelo driver.

# Veja o método funcionando

Copie e cole o código abaixo, dê um Build e execute.
```
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
        private StringBuilder verificationErrors;
        private string baseURL;
        public string localArquivo;
        private bool acceptNextAlert = true;
        int contador = 0;

        //Método para capturar screenshot da tela
        public void Screenshot(IWebDriver driver, string localArquivo)
        {
            ITakesScreenshot camera = driver as ITakesScreenshot;
            Screenshot foto = camera.GetScreenshot();
            foto.SaveAsFile(localArquivo, ImageFormat.Png);
            screenshotsPasta = @"C:\Users\cciola\Desktop\Selenium\Evidencias\";
        }

        [SetUp]
        public void SetupTest()
        {
            driver = new ChromeDriver();
            driver.Manage().Window.Maximize();
            baseURL = "https://www.google.com.br";
            verificationErrors = new StringBuilder();
        }

        public void capturaImagem()
        {
            Thread.Sleep(500);
            Screenshot(driver, screenshotsPasta + "Imagem_" + contador++ + ".png");
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

        public void CadastroPositivo()
        {
              driver.Navigate().GoToUrl(baseURL + "/books");
              Thread.Sleep(1000);
              capturaImagem();
              Thread.Sleep(1000);
        }

        private bool IsElementPresent(By by)
        {
            try
            {
                driver.FindElement(by);
                return true;
            }
            catch (NoSuchElementException)
            {
                return false;
            }
        }

        private bool IsAlertPresent()
        {
            try
            {
                driver.SwitchTo().Alert();
                return true;
            }
            catch (NoAlertPresentException)
            {
                return false;
            }
        }

        private string CloseAlertAndGetItsText()
        {
            try
            {
                IAlert alert = driver.SwitchTo().Alert();
                string alertText = alert.Text;
                if (acceptNextAlert)
                {
                    alert.Accept();
                }
                else
                {
                    alert.Dismiss();
                }
                return alertText;
            }
            finally
            {
                acceptNextAlert = true;
            }
        }

        public string screenshotsPasta { get; set; }
    }
}
```
O código acessa a página base <i>www.google.com.br</i> concatenada ao endereço da URL <i>/books</i>, e captura um screenshot da página após meio segundo.
Ao acessar a pasta informada, resultado será o arquivo com o screenshot salvo com o nome desejado mais a numeração 1.
<br></br>
Dúvidas me contate! carol.ciola@gmail.com
