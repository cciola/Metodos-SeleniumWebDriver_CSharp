# Gerador de data aleatória

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
Após a classe pública do projeto, localizada logo abaixo do [TextFixture], inserimos a função que calcula e gera uma data válida de forma randômica, ou seja, datas diferentes a cada vez que o método GerarData() for declarado.
Abaixo temos a declaração do intervalo de anos, meses e dias que serão utilizados na datetime.date:
```
        //Método para gerar data de nascimento válida de forma randômica - colar dentro de [TestFixture]
        DateTime GerarData()
        {
            Random rnd = new Random();
            int ano = rnd.Next(1950, 2016);
            int mes = rnd.Next(1, 12);
            int dia = DateTime.DaysInMonth(ano, mes);
            int Dia = rnd.Next(1, dia);
            DateTime data = new DateTime(ano, mes, Dia);
            return data;
        }
```
<b>Atenção:</b> no momento de declarar o GerarData() no [Test] para chamar o método, converter antes para string. Exemplo:
```
driver.FindElement(By.Id("MainContent_txtDataNascimento")).SendKeys(Convert.ToString(GerarData()));
```        
# Veja o método funcionando

Copie e cole o código abaixo, dê um Build e execute o teste.
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
        IJavaScriptExecutor js; // Javascript Executor

        //Método para gerar CPF válido de forma randômica - colar dentro de [TestFixture]
        public string GerarCpf()
        {
            int soma = 0, resto = 0;
            int[] multiplicador1 = new int[9] { 10, 9, 8, 7, 6, 5, 4, 3, 2 };
            int[] multiplicador2 = new int[10] { 11, 10, 9, 8, 7, 6, 5, 4, 3, 2 };
            Random rnd = new Random();
            string semente = rnd.Next(100000000, 999999999).ToString();
            for (int i = 0; i < 9; i++)
                soma += int.Parse(semente[i].ToString()) * multiplicador1[i];
            resto = soma % 11;
            if (resto < 2)
                resto = 0;
            else
                resto = 11 - resto;
            semente = semente + resto;
            soma = 0;
            for (int i = 0; i < 10; i++)
                soma += int.Parse(semente[i].ToString()) * multiplicador2[i];
            resto = soma % 11;
            if (resto < 2)
                resto = 0;
            else
                resto = 11 - resto;
            semente = semente + resto;
            return semente;
        }

        [SetUp]
        public void SetupTest()
        {
            driver = new ChromeDriver();
            driver.Manage().Window.Maximize();
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
            js = (IJavaScriptExecutor)driver; //Permite executar Javascript

            driver.Navigate();
            Thread.Sleep(1000);
            js.ExecuteScript("alert('CPF gerado: "+GerarCpf()+"');");
            Thread.Sleep(3000);
            driver.Quit();
        }
    }
}
```
O código abre o navegador e exibe a mensagem "Data gerada: 00/00/0000" via JavascriptExecutor.
<br></br>
Dúvidas me contate! carol.ciola@gmail.com
