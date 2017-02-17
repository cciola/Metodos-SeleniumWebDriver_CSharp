# Exibir destaque de elemento com JavascriptExecutor

Após criar um Unit Test Project e importar as bibliotecas abaixo:
```csharp
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
Na classe pública do projeto, localizada logo abaixo do [TextFixture], informe a seguinte variável:
```csharp
    [TestFixture]
    public class NomeDoProjeto
    {
        public IWebDriver driver;
        private string baseURL;
        IJavaScriptExecutor js; // Javascript Executor
        public string pesquisa = "teste de software";
```
Depois declare a seguinte classe:
```csharp
        public void destaque(IWebElement elemento)
        {
            IJavaScriptExecutor js; // Javascript 
            js = (IJavaScriptExecutor)driver;
            js.ExecuteScript("arguments[0].setAttribute('style', arguments[1]);", elemento, "color: yellow; border: 4px solid yellow;");
            Thread.Sleep(500);
        }
```      
Declare antes do código do teste, no [Test]:
```csharp
js = (IJavaScriptExecutor)driver; //Permite executar Javascript
```
No script de teste, utilizamos os seguintes comandos:
```csharp      
//exibe o popup com um texto fixo informado
destaque(driver.FindElement(By.Id("informe o ID do elemento que deseja destacar")));
```
# Veja o método funcionando - texto fixo

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
    public class TesteGit
    {
        public IWebDriver driver;
        private string baseURL;
        IJavaScriptExecutor js; // Javascript Executor
        public string pesquisa = "teste de software";

        public void destaque(IWebElement elemento)
        {
            IJavaScriptExecutor js; // Javascript 
            js = (IJavaScriptExecutor)driver;
            js.ExecuteScript("arguments[0].setAttribute('style', arguments[1]);", elemento, "color: yellow; border: 4px solid yellow;");
            Thread.Sleep(500);
        }

        [SetUp]
        public void SetupTest()
        {
            driver = new ChromeDriver();
            driver.Manage().Window.Maximize();
            baseURL = "https://www.google.com.br";
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
        public void GerarCPF()
        {
            js = (IJavaScriptExecutor)driver; //Permite executar Javascript

            driver.Navigate().GoToUrl(baseURL);
            Thread.Sleep(2000);

            destaque(driver.FindElement(By.Id("sb_ifc0")));
            driver.FindElement(By.Id("lst-ib")).Click();
            driver.FindElement(By.Id("lst-ib")).SendKeys(pesquisa);
            Thread.Sleep(1000);
            driver.FindElement(By.Id("lst-ib")).SendKeys(Keys.Enter);
            Thread.Sleep(2000);
        }
    }
}
```
O código acima abre o navegador, acessa o site de busca do Google, destaca o campo de pesquisa e efetua uma pesquisa.
<br></br>
Dúvidas me contate! carol.ciola@gmail.com
