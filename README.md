#Email coding tips

In short, nested tables with hacky tricks to make it responsive. And you are back in 1999.

This guide is not comprehensive but good enough to code for major email clients.


##General

###Important points
- Email clients such as Gmail will strip your `<style>` tag in `<head>`
- No standard tags like `<div>`, `<p>`, `<h1>` and `<h2>`
- `colspan` or `rowspan` attributes cannot be used. Use nested tables instead
- Margin not supported
- No shorthand (i.e. `style="font: 8px/14px Arial, sans-serif;"`)
- Specify every single value for padding (i.e. `padding-top: 10px; padding-right: 10px; padding-bottom: 8px; padding-left: 5px;`)
- Specify all your widths. Main container with width in pixels and percentages inside the container works (good for responsive design)
- Full six characters of a hex code
- Set `border: 0;` to `<img>` to remove unwanted borders on linked images
- Multiple classes not allowed
- `style="height: auto"` won't work for images on Outlook. Set the height in pixels. Then use `height: auto!important` for responsive design using a class
- Background images can be used on email body by assigning them to both the `<body>` tag and a wrapper table. On table cells level, VML hack approach needs to be applied http://blog.mailermailer.com/email-design/bulletproof-email-background-images-fact-or-fiction
- VML hack supports `background-repeat` property
- Earlier versions of WebKit-powered mail clients (basically only Apple Mail 6 and below, and Outlook 2011 in some cases) only support max-width on block-level elements
- Use cross-platform fonts such as Arial, Verdana, Georgia, and Times New Roman


###Other points
- `<meta http-equiv="X-UA-Compatible" content="IE=edge" />` is for Windows Phones to render correctly
- use `<b>` tag to create bold text
- set paddings, `line-height` for each cell
- If an attribute exists in HTML, use that instead of CSS (i.e. width)
- 600 pixels is a safe maximum width within most desktop and webmail clients
- Image names should be short and meaningful (i.e. `email.gif`, not `campaign_054_design_0x0_v6_email-link.gif`) otherwise can be treated as spam
- PNG may not be supported by some old email clients (e.g. Lotus 6 and 7)
- The look of alt text (when images are not loaded) can be changed by styling the parent `<td>`
- HTML validation: http://validator.w3.org/


###Code Examples

Use XHTML 1.0 Transitional for the HTML doctype:
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<!--[if !mso]><!-->
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
<!--<![endif]-->
<title>Demystifying Email Design</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
</head>
```


Add a table with a width of 100% inside the `<body>` tag. This acts as a true body tag for our email, because styling of the body tag isn't fully supported
```html
<body style="margin: 0; padding: 0;">
  <table border="1" cellpadding="0" cellspacing="0" width="100%">
    <tr>
      <td>
        <table align="center" border="1" cellpadding="0" cellspacing="0" width="600" style="border-collapse: collapse; -webkit-text-size-adjust: 100%; -ms-text-size-adjust: 100%;">
         <tr>
          <td bgcolor="#70bbd9">
           Row 1
          </td>
         </tr>
         <tr>
          <td bgcolor="#ffffff">
           Row 2
          </td>
         </tr>
         <tr>
          <td bgcolor="#ee4c50">
           Row 3
          </td>
         </tr>
        </table>
      </td>
    </tr>
  </table>
</body>
```


If you want to use margin between elements (not supported), or padding doesn't work:
```html
<tr><td style="font-size: 0; line-height: 0;" height="10">&nbsp;</td></tr>
```


Use `style="display:block;` on images to avoid gaps underneath them:
```html
<td align="center" bgcolor="#70bbd9" style="padding: 40px 0 30px 0;">
 <img src="images/h1.gif" alt="Creating Email Magic" width="300" height="230" style="display: block;" />
</td>
```


Use both CSS and `<font>` tag to style links
```html
<td style="color: #ffffff; font-family: Arial, sans-serif; font-size: 14px;">
 &reg; Someone, somewhere 2013<br/>
 <a href="#" style="color: #ffffff;"><font color="#ffffff">Unsubscribe</font></a> to this newsletter instantly
</td>
```


##Responsive email

###Important points
- Outlook will automatically stack your tables if there isn't at least 25px to spare on any given row (using `align="left"` technique)
- Yahoo! Mail ignores media query conditional code and the fix is to use attribute selector https://www.emailonacid.com/blog/article/email-development/stop_yahoo_mail_from_rendering_your_media_queries


###Code Examples

Alternative solution for the 25px issue with aligned tables instead of changing the width of tables:
```html
<!--[if mso]>
   </td><td>
<![endif]-->
```


`max-width` is not supported in Outlook and Lotus Notes 8 & 8.5. This conditional code targets IE (which is the rendering engine used by Lotus Notes) and Outlook.
```html
<!--[if (gte mso 9)|(IE)]>
<table width="600" align="center" cellpadding="0" cellspacing="0" border="0">
    <tr>
        <td>
            <![endif]-->
            <table class="content" align="center" cellpadding="0" cellspacing="0" border="0">
                <tr>
                    <td>
                        Hello!
                    </td>
                </tr>
            </table>
            <!--[if (gte mso 9)|(IE)]>
        </td>
    </tr>
</table>
<![endif]-->
```


For Apple Mail, which dose not support `max-width`:
```html
<!--[if (gte mso 9)|(IE)]>
<table width="425" align="left" cellpadding="0" cellspacing="0" border="0">
    <tr>
        <td>
        <![endif]-->
            <table class="col425" align="left" border="0" cellpadding="0" cellspacing="0" style="width: 100%; max-width: 425px;">
                <tr>
                    <td height="70">
                    </td>
                </tr>
            </table>
        <!--[if (gte mso 9)|(IE)]>
        </td>
    </tr>
</table>
<![endif]-->
```
```css
.col425 {width: 425px!important;}
```


Button with fluid width:
```html
<table class="buttonwrapper" bgcolor="#e05443" border="0" cellspacing="0" cellpadding="0">
  <tr>
    <td class="button" height="45">
      <a href="#">Claim yours!</a>
    </td>
  </tr>
</table>
```


Making the whole button clickable on mobile:
```css
@media only screen and (max-width: 550px), screen and (max-device-width: 550px) {
  body[yahoo] .buttonwrapper {background-color: transparent!important;}
  body[yahoo] .button a {background-color: #e05443; padding: 15px 15px 13px!important; display: block!important;}
}
```