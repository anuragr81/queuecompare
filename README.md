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
