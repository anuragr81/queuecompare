package appmain;

import java.awt.*;
import java.awt.event.*;
import java.awt.image.*;
import javax.swing.*;

public class ImageShow extends Component {

  public void paint(Graphics g) {
		g.drawImage(m_Image, 0, 0, null);
	}

	private ImageShow(BufferedImage img) {
		m_Image = img;
	}

	public Dimension getPreferredSize() {
		if (m_Image == null) {
			return new Dimension(100, 100);
		} else {
			return new Dimension(m_Image.getWidth(null),
					m_Image.getHeight(null));
		}
	}

	public static void showImage(BufferedImage img) {
		JFrame f = new JFrame("ImageShow");
		f.addWindowListener(new WindowAdapter() {

			public void windowClosing(WindowEvent e) {
				System.exit(0);
			}
		});
		f.add(new ImageShow(img));
		f.pack();
		f.setVisible(true);
	}

	private static final long serialVersionUID = 6388510292689827178L;
	private BufferedImage m_Image;
}




package utils;

import Jama.Matrix;

// https://r-forge.r-project.org/scm/viewvc.php/*checkout*/pkg/fGarch/R/snorm.R?revision=2482&root=rmetrics&pathrev=4173		 
/*

	dsnorm <-  
    function(x, xi) 
{   
    # A function implemented by Diethelm Wuertz 

    # Description:
    #   Compute the density function of the "normalized" skew 
    #   normal distribution
    
    # FUNCTION:

    # Standardize:
    m1 = 2/sqrt(2*pi)
    mu = m1 * (xi - 1/xi)
    sigma = sqrt((1-m1^2)*(xi^2+1/xi^2) + 2*m1^2 - 1)
    z = x*sigma + mu  
    # Compute:
    Xi = xi^sign(z)
    g = 2 / (xi + 1/xi) 
    Density = g * dnorm(x = z/Xi)  
    # Return Value:
    Density * sigma 
}

dsnorm <- 
    function(x, mean = 0, sd = 1, xi = 1.5)
{   
    # A function implemented by Diethelm Wuertz 

    # Description:
    #   Compute the density function of the skew normal distribution
    
    # Arguments:
    #   x - a numeric vector of quantiles.
    #   mean, sd, xi - location parameter, scale parameter, and 
    #       skewness parameter.
    
    # FUNCTION:
    
    # Shift and Scale:
    result = .dsnorm(x = (x-mean)/sd, xi = xi) / sd
    
    # Return Value:
    result
}
	*/


public class SkewedNormalDistr {

	public double pdf(double[] xarr,double xi){
	double m1 = 2/Math.sqrt(Math.PI);
	double mu = m1 * (xi-1/xi);
	
	Matrix mu_mat = new Matrix(xarr.length,1,mu);
	
	Matrix xnew = new Matrix(xarr, xarr.length);
	
	double sigma = Math.sqrt((1-m1*m1)*(xi*xi+1/(xi*xi)) + 2*m1*m1 - 1);
	
	Matrix z = new Matrix(xarr,xarr.length);
	z = xnew.times(sigma).plus(mu_mat);
	
	// Compute:
		    Xi = xi^sign(z)
		    g = 2 / (xi + 1/xi) 
		    Density = g * dnorm(x = z/Xi)  
		    # Return Value:
		    Density * sigma
	return m1;
	}
}
